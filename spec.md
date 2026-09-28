# Spec: Critical Crust, ER-модель

## Намір
Змоделювати дані системи для кондитерів: каталог інградієнтів із замовленнями
та каталог рецептів зі смаками й приміткою про обладнання. Підбір рецептів і
замовлення інградієнтів є незалежними функціями.

## Сутності, атрибути, зв'язки

- **Customer** (CustomerID PK, UserName, Address, PhoneNumber): оформлює 0..N Order.
- **Order** (OrderID PK, CustomerID FK, OrderDate, TotalAmount): належить рівно одному Customer, містить 1..N ItemQuantity.
- **ItemQuantity** (ItemQuantityID PK, OrderID FK, IngredientID FK, Quantity, PriceAtOrder): асоціативна сутність між Order і Ingredient, належить одному Order і одному Ingredient.
- **Ingredient** (IngredientID PK, CategoryID FK, Name, Price, PackSize): товар на складі, належить одній Category, входить у 0..N ItemQuantity.
- **Category** (CategoryID PK, Name, Unit): група товарів, містить 0..N Ingredient, використовується в 0..N RecipeItemQuantity.
- **Recipe** (RecipeID PK, Name, EquipmentNote): має 1..N RecipeItemQuantity і 1..N RecipeTaste.
- **RecipeItemQuantity** (RecipeItemQuantityID PK, RecipeID FK, CategoryID FK, RecipeItemAmount): асоціативна сутність між Recipe і Category, кількість у одиницях Category.Unit.
- **Taste** (TasteID PK, Name): довідник смаків, входить у 0..N RecipeTaste.
- **RecipeTaste** (RecipeTasteID PK, RecipeID FK, TasteID FK, Intensity): асоціативна сутність між Recipe і Taste.

Order і Recipe не пов'язані свідомо.

## Критерії прийняття

1. Кожна сутність має PK, а кожен зв'язок 1:N реалізований через FK на боці «багато».
2. Усі ID (PK і FK) мають тип int. Грошові поля (Price, PriceAtOrder, TotalAmount) мають однаковий тип. PhoneNumber має тип string.
3. M:N виноситься в асоціативну сутність лише коли зв'язок має власні атрибути (ItemQuantity, RecipeItemQuantity, RecipeTaste).
4. Order має ≥1 ItemQuantity. Recipe має ≥1 RecipeItemQuantity і ≥1 RecipeTaste. Ingredient, Category, Taste можуть існувати без зв'язаних записів.
5. Order і Recipe не пов'язані.
6. Усі атрибути атомарні (1NF), смаки й категорії винесені в окремі сутності.
7. Unit зберігається тільки в Category. PriceAtOrder і TotalAmount є знімками на момент замовлення, Ingredient.Price є поточною ціною.
8. Назви сутностей і полів у spec.md збігаються з діаграмою символ у символ.
9. Кожен Ingredient має PackSize (розмір фасування) в одиницях своєї Category.Unit.
10. RecipeItemAmount є десятковим числом, грошові поля мають один і той самий десятковий тип.
11. Intensity набуває значень лише з фіксованого переліку: слабо, помірно, сильно.
