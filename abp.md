# abp

#### Install the ABP CLI <a href="#install-the-abp-cli" id="install-the-abp-cli"></a>

```powershell
dotnet tool install -g Volo.Abp.Cli
```

Create new project

```powershell
mkdir BookStore
cd BookStore
abp new BookStore
```

## Cấu trúc

```
*.Domain.Shared contains
- Constants (BookConsts)
- Enums (BookType)
*.Domain contains
- Entities, aggregate roots (Book)
- Domain services (BookManager)
- Value objects (BookStoreConsts)
- Repository interfaces (IBookRepository)
*.Application.Contracts contains
- Application service interfaces (IBookAppService)
- Data transfer objects (BookCreationDto)
*.Application contains
- Application service implementations (BookAppService)
*.EntityFrameworkCore contains
- *Dbcontext (EntityFrameworkCore/BookStoreDbContext)
- Repository implementations (Authors/EfCoreAuthorRepository)

```

## Các bước để thêm một Entity

### Thêm Entity

Trong project \*.Domain, trong folder Things, tạo class Thing

<details>

<summary>class Thing</summary>

```csharp
using System;
using JetBrains.Annotations;
using Volo.Abp;
using Volo.Abp.Domain.Entities.Auditing;

namespace Acme.BookStore.Authors
{
    public class Author : FullAuditedAggregateRoot<Guid>
    {
        public string Name { get; private set; }
        public DateTime BirthDate { get; set; }
        public string ShortBio { get; set; }

        private Author()
        {
            /* This constructor is for deserialization / ORM purpose */
        }

        internal Author(
            Guid id,
            [NotNull] string name,
            DateTime birthDate,
            [CanBeNull] string shortBio = null)
            : base(id)
        {
            SetName(name);
            BirthDate = birthDate;
            ShortBio = shortBio;
        }

        internal Author ChangeName([NotNull] string name)
        {
            SetName(name);
            return this;
        }

        private void SetName([NotNull] string name)
        {
            Name = Check.NotNullOrWhiteSpace(
                name, 
                nameof(name), 
                maxLength: AuthorConsts.MaxNameLength
            );
        }
    }
}

```

</details>

### Thêm Const (nếu cần)

Trong project \*.Domain.Shared, trong folder Things, tạo class ThingConsts

<details>

<summary>class ThingConsts</summary>

```csharp
namespace Acme.BookStore.Authors
{
    public static class AuthorConsts
    {
        public const int MaxNameLength = 64;
    }
}

```

</details>

### Thêm Manager

Trong project \*.Domain, trong folder Things, tạo class ThingManager

<details>

<summary>class ThingManager</summary>

```csharp
using System;
using System.Threading.Tasks;
using JetBrains.Annotations;
using Volo.Abp;
using Volo.Abp.Domain.Services;

namespace Acme.BookStore.Authors
{
    public class AuthorManager : DomainService
    {
        private readonly IAuthorRepository _authorRepository;

        public AuthorManager(IAuthorRepository authorRepository)
        {
            _authorRepository = authorRepository;
        }

        public async Task<Author> CreateAsync(
            [NotNull] string name,
            DateTime birthDate,
            [CanBeNull] string shortBio = null)
        {
            Check.NotNullOrWhiteSpace(name, nameof(name));

            var existingAuthor = await _authorRepository.FindByNameAsync(name);
            if (existingAuthor != null)
            {
                throw new AuthorAlreadyExistsException(name);
            }

            return new Author(
                GuidGenerator.Create(),
                name,
                birthDate,
                shortBio
            );
        }

        public async Task ChangeNameAsync(
            [NotNull] Author author,
            [NotNull] string newName)
        {
            Check.NotNull(author, nameof(author));
            Check.NotNullOrWhiteSpace(newName, nameof(newName));

            var existingAuthor = await _authorRepository.FindByNameAsync(newName);
            if (existingAuthor != null && existingAuthor.Id != author.Id)
            {
                throw new AuthorAlreadyExistsException(newName);
            }

            author.ChangeName(newName);
        }
    }
}

```

</details>

### Thêm Exception (nếu cần)

Trong project \*.Domain, trong folder Things, tạo class ThingReasonException

<details>

<summary>class ThingReasonException</summary>

```csharp
using Volo.Abp;

namespace Acme.BookStore.Authors
{
    public class AuthorAlreadyExistsException : BusinessException
    {
        public AuthorAlreadyExistsException(string name)
            : base(BookStoreDomainErrorCodes.AuthorAlreadyExists)
        {
            WithData("name", name);
        }
    }
}

```

</details>

### Thêm ErrorCodes (nếu cần)

Trong project \*.Domain.Shared, trong class \*DomainErrorCodes, thêm field ThingReason

<details>

<summary>class *DomainErrorCodes</summary>

```csharp
namespace Acme.BookStore
{
    public static class BookStoreDomainErrorCodes
    {
        public const string AuthorAlreadyExists = "BookStore:00001";
    }
}

```

</details>

### Thêm translation code (nếu cần)

Trong project \*.Domain.Shared, trong file Localization/\*/en.json, thêm code dịch

<details>

<summary>translation code</summary>

```json
"BookStore:00001": "There is already an author with the same name: {name}"

```

</details>

### Thêm IThingRepository

Trong project \*.Domain, trong folder Things, thêm class IThingRepository

<details>

<summary>class IThingRepository</summary>

```csharp
using System;
using System.Collections.Generic;
using System.Threading.Tasks;
using Volo.Abp.Domain.Repositories;

namespace Acme.BookStore.Authors
{
    public interface IAuthorRepository : IRepository<Author, Guid>
    {
        Task<Author> FindByNameAsync(string name);

        Task<List<Author>> GetListAsync(
            int skipCount,
            int maxResultCount,
            string sorting,
            string filter = null
        );
    }
}

```

</details>

### Thêm field DbSet\<Thing> Things

Trong project \*.EntityFrameworkCore, trong folder EntityFrameworkCore, trong file \*DbContext, trong class \*DbContext, thêm field:

<details>

<summary>DbSet&#x3C;Thing> Things</summary>

```csharp
public DbSet<Thing> Things { get; set; }
```

</details>

### Thêm code builder.Entity\<Thing>

Trong project \*.EntityFrameworkCore, trong folder EntityFrameworkCore, trong file \*DbContext, trong class \*DbContext, trong method OnModelCreating, thêm code:

<details>

<summary>builder.Entity&#x3C;Thing></summary>

```csharp
builder.Entity<Author>(b =>
{
    b.ToTable(BookStoreConsts.DbTablePrefix + "Authors",
        BookStoreConsts.DbSchema);
    
    b.ConfigureByConvention();
    
    b.Property(x => x.Name)
        .IsRequired()
        .HasMaxLength(AuthorConsts.MaxNameLength);

    b.HasIndex(x => x.Name);
});

```

</details>

### Thêm Database Migration

Trong project \*.EntityFrameworkCore, mở terminal lên và gõ:

```
dotnet ef migrations add Added_Authors
dotnet ef database update
```

Xem [thêm](https://docs.abp.io/en/abp/latest/Tutorials/Part-7?UI=MVC\&DB=EF#create-a-new-database-migration)

### Thêm EfCoreThingRepository

Trong project \*.EntityFrameworkCore, trong folder Things, tạo class EfCoreThingRepository

<details>

<summary>class EfCoreThingRepository</summary>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Linq.Dynamic.Core;
using System.Threading.Tasks;
using Acme.BookStore.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;
using Volo.Abp.Domain.Repositories.EntityFrameworkCore;
using Volo.Abp.EntityFrameworkCore;

namespace Acme.BookStore.Authors
{
    public class EfCoreAuthorRepository
        : EfCoreRepository<BookStoreDbContext, Author, Guid>,
            IAuthorRepository
    {
        public EfCoreAuthorRepository(
            IDbContextProvider<BookStoreDbContext> dbContextProvider)
            : base(dbContextProvider)
        {
        }

        public async Task<Author> FindByNameAsync(string name)
        {
            var dbSet = await GetDbSetAsync();
            return await dbSet.FirstOrDefaultAsync(author => author.Name == name);
        }

        public async Task<List<Author>> GetListAsync(
            int skipCount,
            int maxResultCount,
            string sorting,
            string filter = null)
        {
            var dbSet = await GetDbSetAsync();
            return await dbSet
                .WhereIf(
                    !filter.IsNullOrWhiteSpace(),
                    author => author.Name.Contains(filter)
                 )
                .OrderBy(sorting)
                .Skip(skipCount)
                .Take(maxResultCount)
                .ToListAsync();
        }
    }
}

```

</details>

### Thêm IThingAppService

Trong project \*.Application.Contracts, trong folder Things, tạo interface:

<details>

<summary>interface IThingAppService</summary>

```csharp
using System;
using System.Threading.Tasks;
using Volo.Abp.Application.Dtos;
using Volo.Abp.Application.Services;

namespace Acme.BookStore.Authors
{
    public interface IAuthorAppService : IApplicationService
    {
        Task<AuthorDto> GetAsync(Guid id);

        Task<PagedResultDto<AuthorDto>> GetListAsync(GetAuthorListDto input);

        Task<AuthorDto> CreateAsync(CreateAuthorDto input);

        Task UpdateAsync(Guid id, UpdateAuthorDto input);

        Task DeleteAsync(Guid id);
    }
}

```

</details>

### Thêm các DTO

Trong project \*.Application.Contracts, trong folder Things, thêm các class:

<details>

<summary>class AuthorDto</summary>

using System; using Volo.Abp.Application.Dtos;

namespace Acme.BookStore.Authors { public class AuthorDto : EntityDto { public string Name { get; set; }

```
    public DateTime BirthDate { get; set; }

    public string ShortBio { get; set; }
}
```

}

</details>

<details>

<summary>class GetAuthorListDto</summary>

```csharp
using Volo.Abp.Application.Dtos;

namespace Acme.BookStore.Authors
{
    public class GetAuthorListDto : PagedAndSortedResultRequestDto
    {
        public string Filter { get; set; }
    }
}

```

</details>

<details>

<summary>class CreateAuthorDto</summary>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

namespace Acme.BookStore.Authors
{
    public class CreateAuthorDto
    {
        [Required]
        [StringLength(AuthorConsts.MaxNameLength)]
        public string Name { get; set; }

        [Required]
        public DateTime BirthDate { get; set; }
        
        public string ShortBio { get; set; }
    }
}

```

</details>

<details>

<summary>class UpdateAuthorDto</summary>

```csharp
using System;
using System.ComponentModel.DataAnnotations;

namespace Acme.BookStore.Authors
{
    public class UpdateAuthorDto
    {
        [Required]
        [StringLength(AuthorConsts.MaxNameLength)]
        public string Name { get; set; }

        [Required]
        public DateTime BirthDate { get; set; }
        
        public string ShortBio { get; set; }
    }
}

```

</details>

### Thêm ThingAppService

Trong project \*.Application, thêm class:

<details>

<summary>class AuthorAppService</summary>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Acme.BookStore.Permissions;
using Microsoft.AspNetCore.Authorization;
using Volo.Abp.Application.Dtos;
using Volo.Abp.Domain.Repositories;

namespace Acme.BookStore.Authors
{
    [Authorize(BookStorePermissions.Authors.Default)]
    public class AuthorAppService : BookStoreAppService, IAuthorAppService
    {
        private readonly IAuthorRepository _authorRepository;
        private readonly AuthorManager _authorManager;

        public AuthorAppService(
            IAuthorRepository authorRepository,
            AuthorManager authorManager)
        {
            _authorRepository = authorRepository;
            _authorManager = authorManager;
        }

        public async Task<AuthorDto> GetAsync(Guid id)
{
    var author = await _authorRepository.GetAsync(id);
    return ObjectMapper.Map<Author, AuthorDto>(author);
}

public async Task<PagedResultDto<AuthorDto>> GetListAsync(GetAuthorListDto input)
{
    if (input.Sorting.IsNullOrWhiteSpace())
    {
        input.Sorting = nameof(Author.Name);
    }

    var authors = await _authorRepository.GetListAsync(
        input.SkipCount,
        input.MaxResultCount,
        input.Sorting,
        input.Filter
    );

    var totalCount = input.Filter == null
        ? await _authorRepository.CountAsync()
        : await _authorRepository.CountAsync(
            author => author.Name.Contains(input.Filter));

    return new PagedResultDto<AuthorDto>(
        totalCount,
        ObjectMapper.Map<List<Author>, List<AuthorDto>>(authors)
    );
}

[Authorize(BookStorePermissions.Authors.Create)]
public async Task<AuthorDto> CreateAsync(CreateAuthorDto input)
{
    var author = await _authorManager.CreateAsync(
        input.Name,
        input.BirthDate,
        input.ShortBio
    );

    await _authorRepository.InsertAsync(author);

    return ObjectMapper.Map<Author, AuthorDto>(author);
}

    [Authorize(BookStorePermissions.Authors.Edit)]
    public async Task UpdateAsync(Guid id, UpdateAuthorDto input)
    {
        var author = await _authorRepository.GetAsync(id);

        if (author.Name != input.Name)
        {
            await _authorManager.ChangeNameAsync(author, input.Name);
        }

        author.BirthDate = input.BirthDate;
        author.ShortBio = input.ShortBio;

        await _authorRepository.UpdateAsync(author);
    }

    [Authorize(BookStorePermissions.Authors.Delete)]
    public async Task DeleteAsync(Guid id)
    {
        await _authorRepository.DeleteAsync(id);
    }

    }
}

```

</details>

### Thêm Object Mapping

Trong project \*.Application, trong class BookStoreApplicationAutoMapperProfile, thêm code sau:

```csharp
CreateMap<Author, AuthorDto>();
```

Hết
