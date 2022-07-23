Розробити API для магазину. Основні функції:
1) Робота з товарами. Можливість додавати показувати, видаляти та змінювати товари
2) Можливість назбирати корзину з товарами і купити їх, або очистити корзину
----------------------------------------------------------------------------------------------------------------------------
Milestone 1(загальне налаштування)
1) В одному солюшині творити проекти:
	- asp.net web api "ShopApi"
	- class library "Services"
	- class library "Repository"
2) Налаштувати залежності:
	- додати в Services посилання на проект Repository
	- додати в ShopApi посилання на проект Services
3) в проекті ShopApi змінити контроллер на TestController, додате метод GET test/ping, у відповідь отримати string: "pong"
4) перевірити TestController запустивши ShopApi проект
----------------------------------------------------------------------------------------------------------------------------
Milestone 2(робота над Repository проектом)
1) Написати клас для товарів Product(string Code, string Name, int Quantity, double Price)
2) Написати клас для конзин Bucket(List<string> Products (список кодів вибраних продуктів), double TotalPrice)
3) Написати статичний клас Storage де будуть три статичні проперті: 
	- List<Product> Products
	- Bucket Bucket
	- double Income(прибуток)
----------------------------------------------------------------------------------------------------------------------------
Milestone 3(робота над Services проектом)
1) додати інтерфейс IProductService з методами 
	- Add(Product), 
	- GetByCode(string code), 
	- GetAll(), 
	- UpdatePrice(string code, double price), 
	- Delete(string code)
2) додати клас ProductService який наслідується від IProductService і реалізувати всі його методи використовуючи Storage.Products
Зверни увагу, що 
	- коли ми додаємо новий товар, то Quantity стає 1. 
	- rоли ми додаємо товар і його код вже є, то ми збільшуємо Quantity + 1, 
	- аналогічно при видаленні, Quantity зменшуємо на 1, і якщо Quantity немає то видаляємо зі списку
3) додати інтерфейс IBucketService з методами та реалізувати їх в класі BucketService:
	- AddToBucket(string productCode), при додаванні продукта в корзину, потрібно витягнути його ціну 
		і в корзині збільшити TotalPrice на ціну цього товару
	- RemoveFromBucket(string productCode) при видаленні товару з корзини, потрібно від TotalPrice відняти ціну продукта
	- Get() показати список продуктів і загальну ціну 
	- Clear() очистити корзину
	- Buy() потрібно збільшини Income на TotalPrice корзини, видали продукти зі списку продуктів бо їх вже купили і очистити корзину
----------------------------------------------------------------------------------------------------------------------------
Milestone 4(робота над ShopApi проектом)
1) Додати контролер ProductController
	- в констурктор додати IProductService
	- додати методи: 
		GET products
		GET products/{code}
		POST products, body{всі поля продукта}
		PUT products/{code} body{price: double}
		DELETE products/{code}
2) Додати контролер BucketController
	- в констурктор додати IBucketService	
	- додати методи:
		GET bucket 
		PUT bucket/add/{productCode}
		PUT bucket/remove/{productCode}
		POST bucket/clear
		POST bucket/buy
3) Зареєструвати сервіси з інтерфейсами в Program.cs через метод AddScoped<IProductService, ProductService> i AddScoped<IBucketService, BucketService>
