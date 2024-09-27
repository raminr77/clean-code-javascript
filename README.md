
<div dir="rtl">

# کدنویسی تمیز در جاوا اسکریپت

## فهرست موضوعات

1. [مقدمه](#مقدمه)
2. [متغیرها](#متغیرها)
3. [توابع](#توابع)
4. [آبجکت ها و ساختارهای داده](#آبجکت-ها-و-ساختارهای-داده)
5. [کلاس ها](#کلاس-ها)
6. [SOLID](#solid)
7. [تست](#تست)
8. [همزمانی](#همزمانی)
9. [ارور هندلینگ](#ارور-هندلینگ)
10. [قالب بندی](#قالب-بندی)
11. [کامنت گذاری](#کامنت-گذاری)
12. [ترجمه](#ترجمه)

## مقدمه

<center>

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

</center>

<br />

در این مطلب، اصول مهندسی نرم افزار برای جاوا اسکریپت از کتاب
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
اثر رابرت سی مارتین برگرفته شده و به عنوان راهنمایی برای نگارش نیست، بلکه راهنمایی برای ایجاد کدهای
[خوانا, قابل استفاده مجدد و قابل تغییر](https://github.com/ryanmcdermott/3rs-of-software-architecture) به زبان جاوا اسکریپت هست.

لازم نیست که هر اصلی که گفته شده حتما رعایت شود و حتی تعداد کمتری از این اصول مورد توافق همه توسعه دهدگان قرار خواهند گرفت. این موارد چیزی جز راهنمای مسیر نیستند و حاصل چندین سال تجربه جمعی نویسندگان _Clean Code_ هستند
.

با اینکه صنعت مهندسی نرم افزار کمی بیش از 50 سال قدمت دارد ولی ما هنوز چیزهای زیادی برای یادگیری داریم. شاید با بالا رفتن قدمت معماری نرم افزار به اندازه خود معماری ، قوانین سخت تری برای پیروی از آن داشته باشیم. الان اجازه دهید این دستورالعمل ها به عنوان یک معیار مهم برای کیفیت سنجی کدی که شما و تیم تان با جاوا اسکریپت تولید می کنید، باشد.

مورد مهمی که باید در نظر داشته باشید ، اینکه دانستن این موارد باعث نمیشه که دیگر هیچ خطایی نداشته باشید و باور کنید که توسعه دهنده قوی تری هستید . برای قوی تر شدن بهتر است که بیشتر کد بزنید و تمرین کنید و فقط روی یک قطعه کد ساده و پیش نویس کار نکنید ، برای مثال خاک رس مرطوب به تنهایی چیز خاصی نیست ولی اگر با فوت کوزه گری به آن حالت بدید و به دفعات کارتان را باز بینی کنید ، قطعا محصول نهایی جذاب تری تولید می کنید.

## **متغیرها**

### از نام گذاری معنادار و قابل تلفظ استفاده کنید

</div>

**بد**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**خوب**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

<div dir="rtl">


### از یک کلمه مشابه برای همان نوع متغیر استفاده کنید

</div>

**بد**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**خوب**

```javascript
getUser();
```

<div dir="rtl">


### از نام های قابل جستجو استفاده کنید

ما بیشتر از اینکه کد می زنیم ، کدها را می خوانیم. خوانا بودن و قابل جستجو بودن کد ما اهمیت بالایی دارد .پس با نام گذاری بی معنی و غیر قابل فهم متغیرها، کسانی که کدمان را میخوانند را آزار ندهیم. از نام های قابل جستوجو استفاده کنیم و برای این منظور ابزارهایی مانند
[buddy.js](https://github.com/danielstjules/buddy.js) و
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
می توانند به شناسایی constant های بدون نام کمک کنند.

</div>

**بد**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**خوب**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

<div dir="rtl">


### از متغیرهای توضیحی استفاده کنید

</div>

**بد**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**خوب**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

<div dir="rtl">


### از نگاشت ذهنی خودداری کنید

متغیرهای صریح بهتر از متغیرهای ضمنی است.

</div>

**بد**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**خوب**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

<div dir="rtl">


### نیاز به تکرار نام شی در متغیرها مرتبط به آن نیست

 اگر نامی برای کلاس یا آبجکت خودتان انتخاب کردید از آنها درنامگذاری متغیر ها استفاده نکنید.

</div>

**بد**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**خوب**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```

<div dir="rtl">


### به جای اتصال کوتاه یا شرطی، از آرگومان های پیش فرض استفاده کنید

آرگومان های پیش فرض اغلب از اتصال کوتاه تمیزترند . توجه داشته باشید که اگر از آنها استفاده کنید، تابع شما فقط مقادیر پیش فرض آرگومان های `undefined` را ارائه می دهد. سایر مقادیر 
"falsy" مثل `''`، `""`، `false`، `null`، `0`، و
`NaN`، با مقدار پیش فرض جایگزین نخواهند شد.

</div>

**بد**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**خوب**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

<div dir="rtl">


## **توابع**

### آرگومان های تابع ( 2 تا یا کمتر در حالت ایده آل)

محدود کردن تعداد آرگومان های یک تابع بسیار اهمیت دارد، چون تست کردن تابع را برایتان آسان تر می کند. داشتن بیش از سه مورد آرگومان باعث سخت تر شدن پروسه تست کدها می شود.

استفاده از یک یا دو آرگومان ایده آل است و در صورت امکان ،در استفاده از سه آرگومان باید خودداری کرد و باید از ادغام کردن استفاده کرد. معمولاً، اگر بیش از دو آرگومان دارید، تابع شما سعی می کند کارهای زیادی را انجام دهد. در مواردی که اینطور نیست ، بیشتر اوقات یک آبجکت سطح بالاتر برای آرگومان کافی است.

 از آنجا که ، جاوا اسکریپت به شما این امکان را می دهد، بدون استفاده از کدهای اضافی که برای ساخت کلاس استفاده می شوند، به راحتی آبجکت بسازید. در صورت نیاز به آرگومان های زیاد در یک تابع می توانید از یک آبجکت استفاده کنید و همه آنها را داخل آن بگذارید.

برای اینکه مشخص شود تابع مورد نظر چه ویژگی هایی را مدنظر دارد، می توانید از destructuring syntax در ES2015 / ES6 استفاده کنید، که چند مزیت اصلی دارد:

1. وقتی کسی به ساختار تابع نگاه می کند، فورا روشن می شود که چه property هایی درون تابع استفاده می شود.
2. می توان از آن برای شبیه سازی named parameters استفاده کرد.
3. Destructuring همچنین از primitive value های مشخص شده از آبجکت که به عنوان آرگومان به تابع پاس داده شده یک کپی ایجاد می کند.  با کمک آن ها می توان از side effectsها جلوگیری کرد و نکته مهم اینکه ،آبجکت ها و آرایه هایی که از آبجکت درون آرگومان یک تابع destructured می شوند ، یک کپی به حساب نمی آیند. 
4. Linter هایی مثل eslint در مورد property های استفاده نشده هشدار می دهند که بدون destructuring غیر ممکن است.

</div>

**بد**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**خوب**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

<div dir="rtl">


### توابع باید یک کار را انجام دهند

این قانون با فاصله زیاد با اهمیت ترین قانون در مهندسی نرم افزار است. وقتی توابع بیش از یک کار انجام می دهند، نوشتن، تست کردن و استدلالشان، سخت تر می شود. وقتی می توانید یک تابع را فقط برای انجام یک کار به صورت ایزوله بنویسید، می توان به راحتی آن را تغییر داد و کد شما بسیار تمیزتر خواهد شد. اگر از کل این راهنما فقط همین یک مورد را استفاده کنید شما از تعداد زیادی از توسعه دهندگان جلو خواهید زد.

</div>

**بد**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**خوب**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

<div dir="rtl">


### نام تابع باید بگوید که چه کاری انجام می دهد

</div>

**بد**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added

addToDate(date, 1);
```

**خوب**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

<div dir="rtl">


### توابع فقط باید در یک سطح abstraction باشند

زمانی که بیش از یک سطح abstraction داشته باشید، معمولا از تابع شما کار زیادی کشیده می شود . تقسیم کردن توابع به قابل استفاده مجدد بودن و تست کردن آسان تر آن منجر می شود.

</div>

**بد**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**خوب**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

<div dir="rtl">


### کد تکراری را حذف کنید

تمام سعی خود را بکنید تا از نوشتن کدهای تکراری خودداری کنید. وجود کد تکراری خوب نیست، چون به این معنی است که وقتی نیاز به تغییر منطق در  برنامه باشد، نیاز به تغییر در بیش از یک نقطه از برنامه را داریم.

تصور کنید در صورتی که یک رستوران را اداره می کنید و موجودی انبار خود از جمله تمام گوجه فرنگی ها ، پیاز ها ، سیر ، ادویه جات و … را پیگیری می کنید و چند لیست دارید که این کار را در آن انجام می دهید و باید همه ی این موجودی ها بعد از سرو یک غذا درست شده با گوجه فرنگی، به روز شوند. در صورتی که اگر یک لیست داشته باشید فقط و فقط یک جا برای آپدیت کردن هست و نه بیشتر.

اغلب اوقات به این علت کدهای تکراری دارید که دو یا چند چیز با تفاوت خیلی جزئی دارید که اشتراکات فراوانی با هم دارند. ولی این تفاوت ها ، شما را مجبور می کند که دو یا چند تابع جداگانه با عملکرد مشابه بسازید. حذف کد تکراری به معنی ایجاد یک abstraction است که می تواند مجموعه ای از موارد مختلف را فقط با یک تابع یا ماژول یا کلاس مدیریت کند.

درست گرفتن abstraction بسیار با اهمیت است، به همین دلیل باید از اصول SOLID مندرج در بخش Class ها پیروی کنید. abstraction های بد ممکن است از کد تکراری بدتر باشد، پس مراقب باشید! با در نظر گرفتن این گفته، اگر می توانید abstraction خوبی انجام دهید، آن را انجام دهید! کدتان را مجددا تکرار نکنید، در غیر این صورت هر زمان که بخواهید یک چیز را ویرایش کنید، باید بخش های مختلفی را تغییر دهید.

</div>

**بد**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**خوب**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

<div dir="rtl">


### آبجکت های پیش فرض را با Object.assign تنظیم کنید

</div>

**بد**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**خوب**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

<div dir="rtl">


### از flagها به عنوان پارامترهای تابع استفاده نکنید

flagها به کاربر شما می گویند که این تابع بیش از یک کار انجام می دهد. توابع باید یک کار انجام دهند. اگر توابع شما براساس مسیرهای وابسته به boolean هستند، توابع خود را به صورت جدا از هم بنویسید .

</div>

**بد**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**خوب**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

<div dir="rtl">


### از Side Effect ها بپرهیزید (بخش 1)

یک تابع در صورتی side effect تولید می کند که کاری به غیر از گرفتن یک مقدار و برگرداندن یک مقدار یا مقادیر دیگر نکند. 
یک  side effect می تواند برای یک فایل نوشته شود
یا برای اصلاح چند متغیر global باشد یا به طرز عجیبی یک مسیر از تمام پول های داخل حسابتان به یک شخص غریبه ایجاد کند.

حالا اگر تحت یک موقعیت که مجبور به داشتن side effect ها در یک برنامه 
بودید مثل مثال قبل که برای یک فایل نوشته بودید، کاری که نیاز هست انجام بدید این هست که به جایی که در آن هستید متمرکز شوید.
از کلاس ها و توابع متعدد در نوشتن یک فایل استفاده نکنید و فقط و فقط یک سرویس بنویسید که این کار را انجام دهد.

نکته اصلی اینجاست که باید از اشتراک گذاری state بین آبجکت های بدون ساختار و استفاده از data type های قابل تغییر که می توانند توسط هر چیزی نوشته شوند و همچنین متمرکز نبودن در جایی که side effect ها رخ می دهند خودداری کرد. اگر شما بتوانید این کارها را انجام بدید، نسبت به خیلی از برنامه نویسان خوشحال تر خواهید بود.

</div>

**بد**

```javascript
// متغیر global که توسط این تابع ارجاع داده شده 
// اگر ما یک تابع دیگر داشته باشیم که از این نام استفاده کند، این نام تبدیل به یک آرایه خواهد شد و می تواند کد ما را break کند

let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**خوب**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

<div dir="rtl">


### از Side Effect ها بپرهیزید (بخش 2)

در جاوااسکریپت ، بعضی مقادیر قابل تغییر هستند و بعضی ها هم غیر قابل تغییرند. آبجکت ها و آرایه ها، 2 نوع قابل تغییر هستند و با احتیاط مدیریت کردن آنها در زمان  پاس دادن به عنوان آرگومان به یک تابع بسیار با اهمیت هست. یک تابع جاوااسکریپت می تواند property های یک آبجکت یا محتویات یک آرایه را تغییر دهد که این موضوع باعث ایجاد باگ هایی در جاهای دیگر می شود.

فرض کنید که یک تابع داریم که با گرفتن یک آرایه به عنوان آرگومان یک سبد خرید ایجاد می کند، برای مثال اگر تابع با اضافه کردن یک آیتم جدید برای خرید در آرایه سبد خرید تغییر کند، هر تابع دیگری که از همین آرایه `cart` استفاده می کند، تغییر می کند.این تغییر همانطور که می تواند خوب باشد به همان اندازه می تواند بد باشد. تصور کنید که در موقعیت بد قرار داریم:

کاربر روی دکمه "Purchase" می زند که تابع `purchase` را که یک درخواست به شبکه را ایجاد می کند و آرایه `cart` را به سرور می فرستد فراخوانی می کند. بدلیل یک درخواست شبکه بد، تابع `purchase` مجبور به تلاش مجدد برای ارسال درخواست می شود.حالا اگر کاربر در همین حین لحظه روی دکمه "Add to cart" به طور اتفاقی برای محصولی که اصلا نمی خواهد قبل از اینکه درخواست شبکه شروع به ارسال کند بزند ، چه اتفاقی می افتد؟ اگر این اتفاق بیافتد و درخواست شبکه شروع شود، آن وقت تابع خرید به صورت تصادفی آیتم اضافه شده را می فرستد ، چون آرایه `cart` تغییر کرده است.

یک راه حل عالی می تواند تابع `addItemToCart` برای کپی کردن `cart`
و ویرایش آ ن و در نهایت بازگرداندن آن کپی باشد. این کار تضمین می کند که توابعی که هنوز از سبد خرید قدیمی استفاده می کنند ، تغییرات در آن ها اعمال نمی شود.

دو هشدار درمورد این نگرش:

1. شاید مواردی باشد که شما واقعا بخواهید که آبجکت ورودی را تغییر بدهید، ولی وقتی شما طبق این نگرش برنامه نویسی پیش می روید آن موارد را خیلی کمیاب تلقی می کنید. در اکثر موارد می توان تغییراتی در آنها ایجاد کرد که هیچ side effect نداشته باشند!

2. کپی کردن آبجکت های بزرگ می تواند در performance برای ما گران تمام شوند، خوشبختانه در عمل این مشکل بزرگی نیست و [کتابخانه های خوبی](https://facebook.github.io/immutable-js/) وجود دارند که به شما اجازه می دهند این نگرش برای شما سریع و با مصرف مموری کمتر و امکان کپی کردن دستی آبجکت ها و آرایه ها را فراهم می کند.

</div>

**بد**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**خوب**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

<div dir="rtl">


### توابع global ننویسید

شلوغکاری با توابع global کار خوبی در جاوااسکریپت نیست چون امکان تقابل با یک کتابخانه دیگر هست و کاربر استفاده کننده از API شما را تا زمان دریافت یک استثنا در production کلافه می کند.بیاید با هم یک مثال را بررسی کنیم:

چی می شود اگر بخواهیم متد آرایه های native در جاوااسکریپت را با یک متد `diff` گسترش بدهیم که می تواند اختلاف بین 2 آرایه را نشان دهد؟ 
شما می توانید یک تابع جدید با  `Array.prototype` ایجاد کنید ولی با یک کتابخانه دیگر که سعی می کند کار مشابه ای را انجام دهد تداخل می کند.چه می شود اگر آن کتابخانه فقط از `diff` برای پیدا کردن اختلاف بین المان های اول و آخر آرایه استفاده کند؟ به همین خاطر هست که بهتر هست که از کلاس در ES2015/ES6 استفاده کنیم و به آسانی `Array` global را گستربش بدیم.

</div>

**بد**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**خوب**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

<div dir="rtl">


### برنامه نویسی functional را به برنامه نویسی imperative ترجیح دهید

جاوااسکریپت در مقایسه با زبان برنامه نویسی haskell یک زبان functional به حساب نمی آید ولی خیلی تمایل به functional بودن دارد.
زبان های functional می توانند بسیار تمیزتر و آسانتر برای تست کردن باشند. اگر می توانید دوستدار این استایل برنامه نویسی باشید.

</div>

**بد**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**خوب**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

<div dir="rtl">


###  دستورات شرطی را کپسوله سازی کنید

</div>

**بد**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**خوب**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

<div dir="rtl">


### از دستورات شرطی منفی استفاده نکنید

</div>

**بد**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**خوب**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

<div dir="rtl">


### از شرطی شدن اجتناب کنید

کار غیر ممکنی به نظر می رسد.در حالی که اولین بار که این را می شنویم ، بیشتر مردم می گویند : "من چطور باید با دستور شرطی `if` کار کنم؟" در جواب باید گفت شما می توانید از polymorphism در اکثر موارد برای انجام این کار استفاده کنید؟ سوال دوم معمولا این است که "این که عالیه ولی من چرا باید بخواهم که با آن کار کنم؟" جواب در مفهوم کد تمیزی هست که اخیرا یاد گرفتیم : اینکه یک تابع باید فقط یک کار انجام دهد وقتی شما کلاس ها یا توابعی که `if` دارند را دارید، به کاربرتان می گویید که تابع شما بیش از یک کار انجام می دهد. فراموش نکنید که با تابع فقط یک کار انجام دهید.

</div>

**بد**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**خوب**

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

<div dir="rtl">


### از type-checking خودداری کنید (بخش 1)
جاوااسکریپت، زیان برنامه نویسی است که در type نداریم، به این معنی که تابع شما می تواند هر نوع داده ای را به عنوان آرگومان بگیرد. گاهی اوقات شما از این آزادی ضربه می خورید و مایلید که از type-checking در تابع خود استفاده کنید. راه های زیادی برای خودداری از این کار هست که اولینش API های سازگار هستند.

</div>

**بد**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**خوب**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

<div dir="rtl">


### از type-checking خودداری کنید (بخش 2)

اگر شما با مقادیر primitive پایه مثل string ها و integer ها و شما نمی توانیداز polymorphism استفاده کنید و نیاز به type-checking را احساس می کنید، باید از تایپ اسکریپت استفاده کنید. یک جایگزین عالی برای جاوااسکریپت معمولی است که برای شما typing را به صورت پیش فرض بر اساس سینتکس جاوااسکریپت اصلی مهیا کرده است. مشکلی که type-checking دستی در جاوااسکریپت دارد این هست که برای انجام درست آن به توابع و کدهای اضافی احتیاج دارد به طوری که type-safety ساختگی شما خوانایی از دست رفته را جبران نمی کند.  کدهای جاوا اسکریپت خود را تمیز نگه دارید، تست های خوبی بنویسید و به  خوبی بازنگری کنید. در غیر این صورت با TypeScript که یک گزینه ی خوب می باشد همه ی این کارها را انجام دهید.

</div>

**بد**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**خوب**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

<div dir="rtl">


### بیش از حد بهینه سازی نکنید

مرورگرهای مدرن بهینه سازی زیادی در بطن خود دارند که در زمان اجرا انجام می دهند. بیشتر اوقات ، بهینه سازی شما چیزی جز اتلاف وقت نیست. [منابع خوبی](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
برای بهینه سازی در وجود دارد که از آنها در زمان درست استفاده کنید

</div>

**بد**

```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**خوب**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

<div dir="rtl">


### کد مرده را حذف کنید

کد مرده به اندازه کد تکراری بد است. دلیلی برای نگهداری آن در کدهایتان ندارید. اگر فراخوانی نمی شود، از شر آن خلاص شوید! کماکان در تارخچه ورژن های شما وجود خواهد داشت و اگر نیاز داشتید براحتی به آن دسترسی خواهید داشت.

</div>

**بد**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**خوب**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

<div dir="rtl">


## **آبجکت ها و ساختارهای داده**

### از getters و setters استفاده کنید

استفاده از getters و setters برای دسترسی به داده های داخل آبجکت می تواند بهتر از دسترسی مستقیم به اعضای آن آبجکت باشد. شاید بپرسید چرا؟ در اینجا به لیستی از دلایل اشاره می کنیم:

- زمانی که می خواهید کارهایی فراتر از به دست آوردن یک property در آبجکت را دارید، نیازی نیست به جستجو و تغییر دسترسی نیست.

- ساده سازی اضافه کردن اعتبارسنجی در زمان `set` کردن.
- ارائه های داخلی را کپسوله سازی می کند.
- به آسانی error handling و logging اضافه کنید.
- در زمان دریافت اطلاعات از سرور می توانید به property های آبجکت خود ویژگی lazy loading اضافه کنید

</div>

**بد**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**خوب**

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

<div dir="rtl">


### در آبجکت هایتان اعضای private بسازید

این ویژگی را می توان از طریق closure ها (برای ES5 و نسخه های پایین تر) ایجاد کرد.

</div>

**بد**

```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**خوب**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

<div dir="rtl">


## **کلاس ها**

### کلاس های ES2015 / ES6 را به توابع ساده ES5 ترجیح دهید

گرفتن ارث بری خوانا ، ساختار و تعاریف متدها با استفاده از کلاس های ES5 سخت هست .اگر به ارث بری نیاز دارید(توجه داشته باشید که ممکن هست اصلا نیاز نداشته باشید) از کلاس های ES6 استفاده کنید. توابع کوچک را به کلاس ترجیح دهید، اما زمانی که احساس کردید به آبجکت های بزرگتر و پیچیده نیاز دارید از کلاس استفاده کنید.

</div>

**بد**

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**خوب**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

<div dir="rtl">


### از chain کردن متد ها استفاده کنید

این الگو در جاوااسکریپت بسیار مفید هست و در بسیاری از کتابخانه ها مثل jQuery و Lodash دیده می شود.این کار به شما اجازه می دهد تا کد خوانا با حجم کمتری داشته باشید. به این دلیل است که می گوییم، از chain کردن توابع استفاده کنید و  و دریابید که کد های شما چقدر تمیز خواهند بود. در توابع کلاس خود به راحتی دستور `this` را می توانید پایان هر تابع بگذارید و متدهای کلاس بیشتری را می توانید chain کنید.

</div>

**بد**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**خوب**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

<div dir="rtl">


### ترکیب کردن را بر ارث بری ترجیح دهید

همانطور که در [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) توسط Gang of Four مطرح شد، در صورت امکان باید ترکیب کردن را به ارث بری ترجیح دهید. دلایل زیاد خوبی هم برای استفاده از ارث بری و هم برای ترکیب کردن وجود دارد . دلیل اصلی برای این کار این هست که اگر ذهنتان به طور غریزی به سمت ارث بری رفت ، به این فکر کنید که آیا ترکیب کردن می تواند بهتر مشکل شما را مدل سازی کند.در برخی موارد این کار شدنی است.

شاید به این فکر بیافتید که "کی باید از ارث بری استفاده کنم؟" این سوال به این بستگی دارد که با چه مشکلی در حال حاضر روبرو هستید .
در اینجا به لیست خوبی از موقعیت هایی اشاره شده که در آن ارث بری نسبت به ترکیب کردن معقولانه تر است:


1. ارث بری شما به یک رابطه ی "is-a" اشاره دارد و نه یک رابطه ی "has-a" (انسان->حیوان vs. کاربر->جزئیات کاربر)

2. می وانید از کدها متعلق به کلاس های اصلی استفاده کنید (انسان می تواند مثل همه حیوانات حرکت کند).

3. زمانی که می خواهید تغییرات کلی در کلاس های وابسته از طریق تغییر در کلاس اصلی ایجاد کنید. (مصرف کالری همه حیوانات را زمانی که حرکت می کنند تغییر دهید).

</div>

**بد**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**خوب**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

<div dir="rtl">


## **SOLID**

### اصل تک مسئولیتی یا (SRP)

همانطور که در Clean Code مطرح شد، "هرگز نباید بیش از یک دلیل برای یک کلاس در تغییر کردن باشد". پر کردن یک کلاس با تعداد زیادی از عملکرد وسوسه بر انگیز هست، مثل زمانی که شما فقط یک چمدان برای پرواز با هواپیما برمیدارد. مشکل این موضوع، این هست که کلاس شما مفهوم منسجمی نخواهد داشت و دلایل زیادی برای تغییر ایجاد میکند به حداقل رساندن تعداد دفعات تغییر دادن یک کلاس بسیار با اهمیت .است. با اهمیت به این دلیل که اگر عملکرد های بسیار زیادی در یک کلاس باشد و شما یک قسمت کوچک آن را تغییر دهید، فهمیدن اینکه این تغییر چطور بر سایر اجزای مستقل در کد شما تاثیر می گذارد سخت می شود.

</div>

**بد**

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**خوب**

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

<div dir="rtl">


### اصل باز/بسته یا (OCP)

همانطور که توسط Bertrand Meyer بیان شد، "موجودیت های نرم افزار (کلاس ها ، ماژول ها، توابع و غیره) باید برای گسترده شدن باز باشند ولی برای تغییرات بسته باشند." حالا این یعنی چه؟ این اصل اساسا بیان می کند که باید به کاربران این اجازه را بدهید که عملکرد های جدید بدون تغییر کد فعلی اضافه کنند.

</div>

**بد**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**خوب**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

<div dir="rtl">


### اصل جانشینی لیسکوو یا (LSP)

این یک اصطلاح ترسناک برای یک مفهوم ساده است.
به صورت رسمی اینگونه تعریف می شود که، "اگر S یک subtype برای T باشد، آن وقت آبجکت های نوع T ممکن است جایگزین آبجکت های نوع S شوند بدون اینکه تغییری در property های آن برنامه ایجاد کنند (اصلاحیه، تسک اجرا شده است). که این خودش تعریف ترسناکتری است."

بهترین توضیحش این است که اگر یک کلاس اصلی دارید و یک کلاس فرزند ، کلاس اصلی و کلاس فرزند می توانند بدون خروجی و نتایج اشتباه بجای هم استفاده شوند. شاید هنوز گیج کننده باشد، بذارید به مثال کلاسیک مربع-مستطیل نگاهی بیاندازیم. از دیدگاه ریاضی ، مربع یک مستطیل است، اما اگر شما آن را با استفاده از رابطه ی "is-a" با ارث بری مدلسازی کنید، شما سریعا به مشکل بر می خورید.

</div>

**بد**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // بد Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**خوب**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

<div dir="rtl">


### اصل تفکیک Interface یا (ISP)

جاوااسکریپت Interfaces ندارد، پس این اصل به طور قطع مانند دیگر اصول صدق نمی کند. با این حال این اصل مهم و مرتبط است حتی با وجود کمبود سیستم type در جاوااسکریپت. 

ISP بیان می کند که "کلاینت ها نباید به وابستگی به interface هایی که استفاده نمی کنند مجبور شوند". Interface ها بدلیل duck typing ها قراردادهای ضمنی در جاوااسکریپت هستند.

یک مثال خوب که این اصل را در جاوااسکریپت نمایش می دهد، برای کلاس هایی است که تنظیمات بزرگی برای آبجکت ها نیاز دارند. نیاز نداشتن به کلاینت ها برای تنظیم گزینه های زیاد با منفعت است، چون اکثر اوقات آنها به همه تنظیمات نیاز پیدا نمی کنند. دلخواه کردن آنها، از داشتن یک "fat interface" جلو گیری می کند.

</div>

**بد**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

**خوب**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

<div dir="rtl">


### اصل وارونگی وابستگی یا (DIP)

این اصل دو چیز اساسی را بیان می کند:

1. ماژول های سطح بالا نباید به ماژول های سطح پایین وابسته باشند، هر دو باید به Abstraction ها بستگی داشته باشند.
2. Abstraction ها نباید به جزئیات بستگی داشته باشد. جزئیات باید به Abstraction ها بستگی داشته باشد.

این موضوع در نگاه اول ممکن است سخت بنظر برسد، اما اگر شما با AngularJS کار کرده باشید، پیاده سازی این اصل در قالب Dependency
Injection (DI) را دیده اید. درحالی که آنها مفاهیم یکسانی نیستند ، DIP ماژول های سطح بالا را از مطلع شدن از جزئیات ماژول های سطح پایین مخفی نگه می دارد. این کار می تواند بوسیله DI محقق شود.  بزرگترین منفعت این کار کاهش coupling بین ماژول هاست. coupling یک الگوی بد برای توسعه هست، چون قابلیت تغییر کدها را سخت می کند.

همانطور که بیان شد، جاوااسکریپت interfaces ندارد، پس abstraction هایی که به آن وابسته هستند ، قراردادهای ضمنی هستند. گفتنی است که متدها و property ها که یک آبجکت یا کلاس را در معرض آبجکت یا کلاسی دیگر قرار می دهند. در مثال زیر، قرارداد ضمنی چیزی است که هر ماژول درخواستی برای ی `InventoryTracker` یک متد `requestItems` خواهد داشت.

</div>

**بد**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // بد We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**خوب**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

<div dir="rtl">


## **تست**

تست کد بسیار مهمتر از انتقال کد می باشد. زمانی که شما هیچ تستی ندارید یا به اندازه کافی تست ندارید، در هر بار انتقال کد به سرور نمی توانید مطمئن شوید که کد شما break نمی شود تصمیم گیری اینکه برا چه اساسی میزان تست نوشته شده کافی است به تیم شما بستگی دارد و ولی پوشش 100% (تمام statement ها و branche ها ) که اعتماد بالا و آرامش خاطر توسعه دهندگان را به همراه دارد این به این معنی است که شما علاوه بر داشتن یک فریمورک تست خوب نیاز به یک [ابزار پوششی خوب](https://gotwarlost.github.io/istanbul/) هم دارید .

هیچ عذر برای ننوشتن تست ها نیست. 
[تعداد زیادی فریمورک تست نویسی برای جاوااسکریپت](https://jstherightway.org/#testing-tools) وجود دارد، یکی از آنهایی که تیم شما ترجیح می دهد را پیدا کنید. وقتی شما فریمورک تستی را پیدا می کنید که در تیم شما کارایی دارد ،هدف گذاری کنید که برای هر feature یا module جدید که معرفی می کنید تست نوشته شود. اگر متد ترجیح داده توسط شما روش Test Driven Development یا (TDD) می باشد ، بسیار عالی است ، اما هدف اصلی اطمینان حاصل کردن از رسیدن و پوشش اهداف تعیین شده قبل از ارائه هر feature و یا تغییر دادن feature های موجود می باشد.


### یک مفهوم در هر تست

</div>

**بد**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**خوب**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

<div dir="rtl">


## **همزمانی**

### بجای callback ها از promise ها استفاده کنید

callback ها تمیز نیستند، و باعث ایجاد تو در تویی بیش از حد می شود.
در ES2015/ES6 ویژگی Promise به عنوان global type از پیش ساخته شده می باشد، از آنها استفاده کنید!

</div>

**بد**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**خوب**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

<div dir="rtl">


### Async / Await حتی تمیزتر از Promiseها 

Promise جایگزین های تمیزی برای callback ها هستندو ولی در ES2017/ES8 ویژگی Async / Await اضافه شد که می تواند راه حل تمیزتری را پیشنهاد کند. تمام چیزی که نیاز دارید یک تابع با کلمه کلیدی به عنوان پیشوند `async` می باشد و سپس می توانید منطق کد خودرا بدون زنجیره `then` بنویسید. اگر می توانید همین امروز از منفعت ویژگی های ES2017/ES8 بهره مند شوید.

</div>

**بد**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**خوب**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

<div dir="rtl">


## **ارور هندلینگ**

خطاهای ایجاد شده چیز خوبی هستند! آنها به این معنی هستند که در زمان اجرا مشکلی در برنامه بوجود آمده را با موفقیت شناسایی کرده است و با متوقف کردن اجرای تابع و از بین بردن process در Node ، شمارا در جریان می گذارد و با stack trace در کنسول به شما خبر می دهد.

### ارور های دریافتی توسط catch را نادیده نگیرید

از کنار خطا هایی که با catch گرفته شدند گذشتن به شما توانایی بطرف کردن یا واکنش به ارور ایجاد شده را نمی دهد. ورود خطا به کنسول (console.log) خیلی خوب نیست، زیرا اغلب اوقات ممکن است در دریایی از چیزهای چاپ شده در کنسول گم شود. اگر هر بیتی از کد را در try / catch قرار دهید، به این معنی است که فکر می کنید ممکن است در آنجا خطایی رخ دهد و بنابراین باید برنامه ای تنظیم کنید یا یک مسیر کد برای زمان بروز آن ایجاد کنید.

</div>

**بد**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**خوب**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

<div dir="rtl">

### promise های رد شده را نادیده نگیرید

به همین دلیل نباید از ارورهای گرفته شده با try / catch چشم پوشی کنید.

</div>

**بد**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**خوب**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

<div dir="rtl">


## **قالب بندی**

قالب بندی یک عمل ذهنی است. مانند بسیاری از قوانین در اینجا، هیچ قانون سخت و سریعی وجود ندارد که شما باید از آن پیروی کنید. نکته اصلی این است که در مورد قالب بندی بحث نکنید. [ابزارهای زیادی](https://standardjs.com/rules.html) برای خودکار کردن این کار وجود دارد. یکی از آن ها را استفاده کنید! بحث و جدال مهندسان درباره قالب بندی، اتلاف وقت و هزینه است. 

برای مواردی که تحت عنوان قالب بندی خودکار قرار نمی گیرند. (تورفتگی، tabها در مقابل spaceها، ” یا ‘ و …)

### از حروف بزرگ استفاده کنید

جاوااسکریپت type ندارد، پس با حروف بزرگ نوشتن به شما چیزهای زیادی در مورد متغیرها و توابع و غیر می گوید. این قوانین ذهنی هستند، پس تیم شما می تواند هر آنچه را که  می خواهد انتخاب کند. مهم نیست که همه شما چه چیزی را انتخاب می کنید، فقط باید در این موضوع ثابت قدم باشید.

</div>

**بد**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**خوب**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

<div dir="rtl">


### توابع تماس گیرنده(callers) و تماس گرفته شده (callees) باید به هم نزدیک باشند

وقتی یک تابع تابع دیگری را صدا می زند، آن دو تابع را به صورت عمودی درون فایل سورس نزدیک هم نگه دارید. به طور ایده آل ، تابع caller را بالای تابع callee نگه دارید.
ما بیشتر مایلیم کد را از بالا به پایین بخوانیم مثل یک روزنامه . بخاطر این موضوع، کد شما هم همانطور خوانده می شود.

</div>

**بد**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**خوب**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

<div dir="rtl">


## **کامنت گذاری**

### فقط چیزهایی را کامنت کنید که منطق بیزنسی پیچیده ای دارند

کامنت ها یک ضرورت نیستند. یک کد خوب در اکثر مواقع خودش را داکیومنت می کند و نیازی به کامنت ندارد.

</div>

**بد**

```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**خوب**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

<div dir="rtl">


### کد های کامنت شده را از کد اصلی خود حذف کنید

ورژن کنترل به این دلیل وجود دارد. کدهای قدیمی را از تاریخچه دنبال کنید.

</div>

**بد**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**خوب**

```javascript
doStuff();
```

<div dir="rtl">


### کامنت های ژورنالی نذارید

به یاد داشته باشید که از ورژن کنترل استفاده کنید! نیازی به کد مرده ، کدهای کامنت شده و کامنت های ژورنالی نیست. از `git log` برای اطلاع از تاریخچه استفاده کنید.

</div>

**بد**

```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**خوب**

```javascript
function combine(a, b) {
  return a + b;
}
```

<div dir="rtl">


### از نشانگرهای موقعیتی خودداری کنید

آنها معمولا فقط کدمان را شلوغ می کنند. اجازه دهید که نام توابع و متغیرها دارای تورفتگی و فرمت مناسب باشند تا ساختار دیداری مناسبی را در کد شما ایجاد کنند.

</div>

**بد**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**خوب**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```
