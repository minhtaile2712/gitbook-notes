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

## Some note

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

```
NewThingConsts in *.Domain.Shared/NewThings
NewThingManager in *.Domain/NewThings
NewThingFirstException in *.Domain/NewThings
*DomainErrorCodes in .Domain.Shared
Localization//en.json in *.Domain.Shared
INewThingRepository in *.Domain/NewThings
add DbSet property in *DbCondtext in *.EntityFrameworkCore
add builder.Entity(b => {}) in OnModelCreating in *DbContext
add db migration new EfCoreNewThingRepository in *.EntityFrameworkCore/NewThings
```

## Các bước để thêm một Entity

Thêm Entity

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

Thêm Const (nếu cần)

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

Thêm Manager

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

Thêm Exception (nếu cần)

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

Thêm ErrorCodes (nếu cần)

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

Thêm code dịch (nếu cần)

Trong project \*.Domain.Shared, trong file Localization/\*/en.json, thêm code dịch

<details>

<summary>code dịch</summary>

```json
"BookStore:00001": "There is already an author with the same name: {name}"

```

</details>

Thêm IThingRepository

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

Thêm DbSet property Things
