# ASP.NET Core and .NET notes from the ECommerce app development course

These are just some miscellaneous notes on ASP.NET Core and .NET from the ECommerce app.

## Repository pattern

> The repository pattern aims to abstract our data access methods away from our controllers, making them thinner, it sits between our DbContext and the controller.

It decouples business code (controllers) from the data access code (DbContext). If we inject the DbContext in every controller, then we'll have similar logic and repeated code for all common operations (imagine we need to retrieve the list of "Product" instances in multiple controller), that's why we need reusable methods coming from the repository.

Example without the repository pattern. **Note how we are using the DbContext to access the DB directly in the controller class**:

```csharp
[Route("api/[controller]")]
     public class ProductsController : ControllerBase
     {
         private readonly StoreContext _context;
         public ProductsController(StoreContext context)

         public ProductsController(IProductRepository productRepository)
         {
             _context = context;
         }

         [HttpGet]
         public async Task<ActionResult<IEnumerable<Product>>> GetProducts()
         {
             return Ok(await _context.Products.ToListAsync());
         }

         [HttpGet("{id:int}")]
         public async Task<ActionResult<Product>> GetProduct(int id)
         {
             return Ok(await _context.Products.FindAsync(id));
         }

     }
```

<div style="page-break-after: always;"></div>

Instead of injecting the DbContext in the controller, we can create and inject a Product repository that contains the methods necessary to retrieve the list of products from the DB and get a product by its Id, and wherever these operations are required, we can inject again the Product repository to avoid repeated code and not mixing the business code with the code related to accessing the information from the DB, which can get quite messy at times.

After implementing the repository pattern. **Note how we have abstracted away the DB access code into our repository service**:

```csharp
[Route("api/[controller]")]
     public class ProductsController : ControllerBase
     {
         private readonly IProductRepository _productRepository;
 
         public ProductsController(IProductRepository productRepository)
         {
             _productRepository = productRepository;
         }

         [HttpGet]
         public async Task<ActionResult<IEnumerable<Product>>> GetProducts()
         {
             var products = await _productRepository.GetProductsAsync();
             return Ok(products);
         }

         [HttpGet("{id:int}")]
         public async Task<ActionResult<Product>> GetProduct(int id)
         {
             var product = await _productRepository.GetProductByIdAsync(id);
             return Ok(product);
         }

     }
```

<div style="page-break-after: always;"></div>

The product repository class. This one is a very silly example because this repository has only two methods, but the main idea from this example is to explain that basically the repository pattern abstracts the DB access code away from the controller to make it more lightweight, also making this DB access code maintainable (it's in a single file) and reusable, **all controllers that require this common DB acess code will only have to add the repository service**:

```csharp
using Core.Entities;
using Core.Interfaces;
using Microsoft.EntityFrameworkCore;

namespace Infrastructure.Data
{
    public class ProductRepository : IProductRepository
    {
        private readonly StoreContext _context;

        public ProductRepository(StoreContext context)
        {
            _context = context;
        }

        public async Task<Product> GetProductByIdAsync(int id)
        {
            return await _context.Products.FindAsync(id);
        }

        public async Task<IReadOnlyList<Product>> GetProductsAsync()
        {
            return await _context.Products.ToListAsync();
        }
    }
```

<div style="page-break-after: always;"></div>

## The generic repository anti-pattern and the specification pattern

> A generic repository, as the name implies, is a repository that is created as a generic, we can provide the class we want to interact with as the parameter, this is considered an anti-pattern, because a repository is an abstraction to begin with, so a generic repository would be the abstraction of an abstraction.

**Using a basic generic repository, we lose the ability to eager load the related entities of our class, since we can't use the "Include" method as usual, because at definition the respository is generic, before we instantiate it passing it a type.**

Example of a generic repository:

```csharp
using Core.Entities;
using Core.Interfaces;
using Core.Specifications;
using Microsoft.EntityFrameworkCore;

namespace Infrastructure.Data
{
    public class GenericRepository<T> : IGenericRepository<T> where T : BaseEntity
    {
        private readonly StoreContext _context;
        public GenericRepository(StoreContext context)
        {
            _context = context;
        }

        public async Task<T> GetByIdAsync(int id)
        {
            return await _context.Set<T>().FindAsync(id);
        }
        public async Task<IReadOnlyList<T>> ListAllAsync()
        {
            return await _context.Set<T>().ToListAsync();
        }
    }
}
```

<div style="page-break-after: always;"></div>

To deal with this issue of not being able to eager load related entities due to our repository being generic, we can use the specification pattern.

> The specification pattern describes a query in an object and it returns an **IQueryable\<T>**

ISpecification interface. The interface for the specification class has two properties, the criteria, which is the expression that must return true for filtering the retrieved objects with the generic repository (this way we can retrieve an object by Id for example), and the list of include expressions, to eager load as many related entities as necessary:

```csharp
namespace Core.Specifications
{
    public interface ISpecification<T>
    {
        Expression<Func<T, bool>> Criteria {get;}
        List<Expression<Func<T, object>>> Includes {get;}
    }
}
```

Creating a base specification class from the ISpecification interface. **Just like the interface, it's a generic class, because the criteria expression and the include expressions depend on the class that is povided as the type**:

```csharp
namespace Core.Specifications
{
    public class BaseSpecification<T> : ISpecification<T>
    {
        public BaseSpecification()
        {
        }

        public BaseSpecification(Expression<Func<T, bool>> criteria)
        {
            Criteria = criteria;
        }

        public Expression<Func<T, bool>> Criteria {get;}
        public List<Expression<Func<T, object>>> Includes {get;} = 
            new List<Expression<Func<T, object>>>();

        protected void AddInclude(Expression<Func<T, object>> includeExpression)
        {
            Includes.Add(includeExpression);
        }
    }
}
```

<div style="page-break-after: always;"></div>

Adapting the generic repository to use the specification class. **Note the private ApplySpecification method, which receives a specification object as a parameter, it passess that specification and also the context set for the entity (T) as a queryable to the SpecificationEvaluator**:

```csharp
namespace Infrastructure.Data
{
    public class GenericRepository<T> : IGenericRepository<T> where T : BaseEntity
    {
        
        private readonly StoreContext _context;
        public GenericRepository(StoreContext context)
        {
            _context = context;
        }

        public async Task<T> GetByIdAsync(int id)
        {
            return await _context.Set<T>().FindAsync(id);
        }
        public async Task<IReadOnlyList<T>> ListAllAsync()
        {
            return await _context.Set<T>().ToListAsync();
        }

        public async Task<T> GetEntityWithSpec(ISpecification<T> spec)
        {
            return await ApplySpecification(spec).FirstOrDefaultAsync();
        }

        public async Task<IReadOnlyList<T>> ListAsync(ISpecification<T> spec)
        {
            return await ApplySpecification(spec).ToListAsync();
        }

        private IQueryable<T> ApplySpecification(ISpecification<T> spec)
        {
            return SpecificationEvaluator<T>
            .GetQuery
            (
                _context.Set<T>().AsQueryable(), 
                spec
            );
        }
    }
}
```

<div style="page-break-after: always;"></div>

Now looking at the SpecificationEvaluator class. To the queryable it received (**_context.Set<T>().AsQueryable()**), it's applying the criteria expression if it's not null in the specification given. It also applies all the include expression to the queryable that are in the include list from the specification. **Finally, it's returning the queryable filtered with the Where method, using the criteria expression, and with the Include method applied as well with the expressions coming from the includes list, all from the specification. This final queryable object is then executed against the DB back in the generic repo as already seen in the previous code extract**:

```csharp
using Core.Entities;
using Core.Specifications;
using Microsoft.EntityFrameworkCore;

namespace Infrastructure.Data
{
    public class SpecificationEvaluator<TEntity> where TEntity : BaseEntity
    {
        public static IQueryable<TEntity> GetQuery(IQueryable<TEntity> inputQuery, ISpecification<TEntity> spec)
        {
            var query = inputQuery;

            if (spec.Criteria != null)
            {
                query = query.Where(spec.Criteria);
            }

            query = spec.Includes.Aggregate(query, (current, include) => current.Include(include));

            return query;
        }
    }
}
```

<div style="page-break-after: always;"></div>

Now, a specification designed to retrieve all Products or a Product by Id, including the Product Brand and Product Type, it works for both cases because of the two constructors we have available, it derives from BaseSpecification of course, note the explanatory and meaningful name:

```csharp
using Core.Entities;

namespace Core.Specifications
{
    public class ProductsWithTypesAndBrandsSpecification : BaseSpecification<Product>
    {
        public ProductsWithTypesAndBrandsSpecification()
        {
            AddInclude(x => x.ProductType);
            AddInclude(x => x.ProductBrand);
        }

        public ProductsWithTypesAndBrandsSpecification(int id) : base(x => x.Id == id)
        {
            AddInclude(x => x.ProductType);
            AddInclude(x => x.ProductBrand);
        }
    }
}
```

Finally, using this specification in the Products controller, for both cases:

```csharp
[HttpGet]
public async Task<ActionResult<IReadOnlyList<ProductToReturnDTO>>> GetProducts() 
{
    var spec = new ProductsWithTypesAndBrandsSpecification();
    
    var products = await _productsRepo.ListAsync(spec);
    
    return Ok(_mapper.Map<IReadOnlyList<ProductToReturnDTO>>(products).ToList());
}

[HttpGet("{id:int}")]
public async Task<ActionResult<ProductToReturnDTO>> GetProduct(int id) 
{
    var spec = new ProductsWithTypesAndBrandsSpecification(id);
    
    var product = await _productsRepo.GetEntityWithSpec(spec);
    return _mapper.Map<ProductToReturnDTO>(product); 
}
```

<div style="page-break-after: always;"></div>

## Deferred execution

> Deferred execution means that an expression or query won't be evaluated until the query or expression is completed in its construction and its realized value is actually required. It improves performance by avoiding unnecessary execution.

**I won't actually execute the query until I'm done adding all the filters, includes and additional conditions for retrieving only the desired information**

- Query commands are stored in a variable
- Execution of the query is deferred
- IQueryable<T> creates an expression tree
- Execution of the query actually takes place when executing ToList(), ToArray(), ToDictionary(), Count(), etc 


