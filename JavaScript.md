# JavaScript.
### Вопросы для собеседования.

### *Junior*
#### Основы основ

    const a = { x : 10 }
    let b = a;
    b['y'] = '20';
    
    console.log(b) // ?
    console.log(a) // ?

**Ответ**: в обоих случах в консоль будет выведен один и тот же объект - `{ x: 10, y : '20' }`
Почему: b является ссылкой на объект a. Изменяя b, мы изменили a.

Почему мы смогли изменить a, ведь она const? Потому что мы не меняли сам объект, мы лишь изменили его свойства, добавив y
Вот если бы мы попробовали сделать, например
    a = { x : 15 }
мы получим ошибку, несмотря на то, что присваемый объект одинаков по структуре с изначальным a

#### В чем отличие конструкций ниже?

    const func1 = () => { }
    
    function func2() { }

**Ответ**: первая - стрелочная функция, присвоенная константе. Она не изменяет контекст выполнения при вызове.
**Вторая** - стандартная функция - при вызове она переключит контекст выполнения на обьъект, её вызвавший.


#### Продолжение. Даны функции и объект, содержащий их:
    function func01() {
        console.log(this);
    }
    
    const func02 = () => {
        console.log(this);
    }
    
    const obj = {
        f1: func01,
        f2: func02
    }

    что выведет вызов obj.f1() и obj.f1() ?
Считаем, что выполнение производится из консоли браузера.

**Ответ**: в первом случае выведется obj, во втором Window.

### Даны два массива объектов - массив teams (команды) и employees (сотрудники). 
Найти среднюю зарплату по отделам и самого высокооплачиваемого сотрудника
    
    const teams = [
        {
            name: 'Development',
            personal: [1, 3]
        },
        {
            name: 'DevOps',
            personal: [1, 3, 4]
        },
        {
            name: 'Administ',
            personal: [1, 2, 3, 4]
        },
    ]
    const employees = [
        {
            id: 1,
            name: "John",
            salary: 200,
            age: 25
        },
        {
            id: 2,
            name: "Mark",
            salary: 285,
            age: 31
        },
        {
            id: 3,
            name: "Tom",
            salary: 180,
            age: 21
        },
        {
            id: 4,
            name: "Sam",
            salary: 275,
            age: 33
        }
    ];

    getStats(employees) => { top_employee: 'Mark', avg_team_salary: 235 }
    getStats(employees, 'DevOps') => { top_employee: 'Sam', avg_team_salary: 471.66 }

Пример реализации:

    function getStats(employees, teamName = null) {
        let localEmployees = employees;
        let topMember = null;
        let maxSalary = 0;
        let avgSalary = 0;
        let salarySum = 0;

        if (teamName) {
            const targetTeam = teams.find((team) => team.name === teamName);
    
            localEmployees = targetTeam.personal.map((member) =>
                employees.find((el) => el.id === member)
            );
        }
    
        localEmployees.forEach((member) => {
            if (member.salary > maxSalary) {
                topMember = member;
                maxSalary = member.salary;
            }
            salarySum += member.salary;
        });
    
        avgSalary = +(salarySum / localEmployees.length).toFixed(2);
    
        return {
            top_employee: topMember.name,
            top_salary: topMember.salary,
            avg_team_salary: avgSalary
        };
    }

### Избавиться от повторяющихся запятых

    const cleared = clearCommas('10:30, 20 jan, ,,,, of 2023,,, ,, wednesday ,, ')
    console.log(cleared)

    // "10:30, 20 jan, of 2023, wednesday "  

Пример реализации:

    function clearCommas(text){
        return Array.from(new Set(text.split(","))).filter(i => i !== " " && i).join(",")
    }

### Проверка правильности закрытия скобок.
Написать функцию, которая определит, что переданная на вход строка содержит валидно закрытые скобки. Пример:

    function checkBraces(inputArray) {  }
    // checkBraces('((()))()') => true
    // checkBraces('((())))') => false

#### Ожидаемый подход: использовать стэк.
1. Разбить строку на массив, затем последовательно идти слева направо, считая закрытые и открытые скобки.
2. Создадим служебный массив. Назовем его stack_array.
3. Если появляется закрытая скобка ) - добавлять её в stack_array.
4. Если после закрытой скобки появляется открытая (  - убирать один элемент из stack_array.
5. Если появляется открытая скобка, а stack_array пуст - возвращаем сразу false.
6. Если дойдя до конца исходного массива окажется, что stack_array пуст - возвращаем true


### Знания очереди выполнения.
Что выведется в консоль при последовательном вызове f1(); f2(); f3(); ?

    function f1() {
        setTimeout(() => console.log('1'), 100)
    }
    
    function f2() {
        console.log('2')
    }
    
    function f3() {
        setTimeout(() => console.log('3'), 0)
    }

Ответ:

    2
    3
    1

Почему? Потому что setTimeout() автоматически переместит вызов функции в конец очереди выполнения, даже если вызван с задержкой 0
Дальше все просто - функция без таймаута выполнится первой, затем функция с нулевым таймаутом, и последней выполнится с таймаутом 100


## Middle

#### Дан массив с различными данными.
#### Написать функцию filterNums(arr), которая выберет в нем только неповторяющиеся числа.
#### Результат вернуть в виде массива, отсортированного по возрастанию
    const arr = [1, 2, 77, '*', '10', 6, 'guyi', 9, 10, '2', 1, 'a']

Ожидаемое решение:

    const filterNums = (arr) => 
        [...new Set(arr.filter(a => !isNaN(a))
            .map(a => +a)
            .sort((a,b) => a - b)))];

    // filterNums(arr) => [ 1, 2, 6, 9, 10, 77]

### Написать функцию parseURL(url), которая разберет URL в массив
Правила: если в части URL встречается знак _, то эта часть игнорируется
Если в части URL встречается знак -, то эта часть разбивается на части по знаку -

    const url = "http://project.com/main/task_details/task/task-22.html"
    
    function parseURL(url) { }
    
    // parseURL(url) => ["project.com", "main", "task", "22"]
    
Пример реализации:

    function parseURL(url) {
        let str_sliced = url.split("//")[1].split("/");

        let result = str_sliced.map((item, index) => {
    
            if (item.indexOf('_') > -1)
                item = item.split('_')[0];
    
            if (index === str_sliced.length - 1)
                item = item.split('.')[0].split("-")[1];
    
            return item;
        });
    
        return result;
    }


### Написать функцию getRanges(arr), которая на вход получит массив чисел.
Необходимо вернуть массив строк, сгруппировав числа исходного массива, которые идут по порядку.

Пример: на вход передан массив 

    [1,2,3,5,6,7]
Результат выполнения функции: 

    ['1-3', '5', '6-7']

Пример реализации:

    let ranges = [2, 3, 8, 9, 10, 11, 13, 15, 16, 17]

    const getRanges = (arr) => {
        const result = [];
        let tmp = [];
        let savedIndex = 0;

    for (let i = 0; i < arr.length - 1; i++) {
        if (arr[i + 1] === arr[i] + 1) {
        } else {
            tmp.push(i);
        }
    }

    for (let i = 0; i < tmp.length; i++) {
        result.push(arr.slice(savedIndex, tmp[i] + 1));
        savedIndex = tmp[i] + 1;
    }

    if (savedIndex !== arr.length) {
        result.push(arr.slice(savedIndex, arr.length));
    }

    return result.map(item => item.length === 1
        ? item[0]
        : `${item[0]}-${item[item.length - 1]}`)
        .join(',')
    };

     console.log(getRanges(ranges)); => ['2','3','8-10','11','13','15-17']

### Необходимо реализовать функцию fetchUrl, которая будет использоваться следующим образом.
 Внутри fetchUrl можно использовать условный метод fetch, который просто возвращает
 Promise с содержимым страницы или вызывает reject
 сatch должен сработать только после 5 неудачных попыток получить содержимое страницы внутри fetchUrl


    function fetchUrl(url, attempt = 5) {
        return Promise.resolve()
            .then(() => fetch(url))
            .catch(() => attempt--
                ? fetchUrl(url, attempt)
                : Promise.reject('rejected after 5 attempts'))
    }
    
    let promise = new Promise(function (resolve, reject) {
        resolve(123);
    });


    promise.then((result) => console.log(result)).catch();
    
    // fetchUrl('https://snapix.ru/')
    //     .then(() => console.log('OK!'))
    //     .catch((e) => console.error(e));
