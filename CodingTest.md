### Questions
```javascript=
const factories = [
  { name: "BR1", employees: ["John", "Alice", "Bob", "Jessie", "Karen"] },
  { name: "BR2", employees: ["Jessie", "Karen", "John"] },
  { name: "BR3", employees: ["Miles", "Eric", "Henry", "Bob"] },
  { name: "BR4", employees: [] }
]; //
```

1. Count Employees Number by Factory // => [ {name: 'BR1', count: 4}, ... ]

```javascript=

/**
 * @typedef {Object} factoryInfo
 * @property {String} name - name of factory
 * @property {Number} count - number of employees at factory
 */

/**
 * Counts Number of Employees for Each Factory.
 * @return {factoryInfo[]}
 */
function countEmployeesNumberByFactory() {
    let result = [];
    for (const item of factories) {
        let it = {name: item.name, count: item.employees.length };
        result.push(it);
    }
    return result;
}
```
2. Count Factories Number by Employee // => [ {employee: 'John', count: 2}, ... ]

```javascript=
/**
 * @typedef {Object} employeeInfo
 * @property {String} employee - name of employee
 * @property {Number} count - number of factories employee works at
 */

/**
 * Counts Number of Factories for Each Employee.
 * @return {employeeInfo[]}
 */
function countFactoryNumberByEmployee() {
    let rMap = {};
    for (const item of factories) {    
        for(const emp of item.employees){
            if(rMap[emp] === undefined){
                rMap[emp] = 1;        
            }else{
                rMap[emp]++;        
            }        
        }
    }
    let result = [];
    for (const key in rMap) {
        let obj = {employee: key, count: rMap[key]}
        result.push(obj);
    }
    return result;
}
```

3. Order employees list by alphabetical order // =>   { name: "BR2", employees: ["Jessie", "John", "Karen"] }
```javascript=
/**
 * @typedef {Object} factoryInfoSorted
 * @property {String} name - name of factory
 * @property {String[]} employees - array of employees sorted in alphabetical order
 */

/**
 * Order employees list within each factory in alphabetical order.
 * @return {factoryInfoSorted[]}
 */
function orderAlphabetical() {
    let result = [] 
    for (let const of factories) {    
        let it = { name: item.name , employees : item.employees.sort() };
        result.push( it ) ;
    }
    return result;
}
```


```javascript=
const employeeType = [
      {id: 1, "name": "FullTime", work_begin: "09:00:00", work_end: "17:00:00"},
      {id; 2, "name": "MidTime", work_begin: "12:00:00", work_end: "21:00:00"},
      {id: 3, "name": "HalfTime", work_begin: "20:00:00", work_end: "00:00:00"},
];

const employees = [
        {id: 1, name: "Alice", type: 2},
        {id: 2, name: "Bob", type: 3},
        {id: 3, name: "John", type: 2},
        {id: 4, name: "Karen", type: 1},
        {id: 5, name: "Miles", type: 3},
        {id: 6, name: "Henry", type: 1}
];

const tasks = [
      {id: 1, title: "task01", duration: 60 //min},
      {id: 2, title: "task02", duration: 120},
      {id: 3, title: "task03", duration: 180},
      {id: 4, title: "task04", duration: 360},
      {id: 5, title: "task05", duration: 30},
      {id: 6, title: "task06", duration: 220},
      {id: 7, title: "task07", duration: 640},
      {id: 8, title: "task08", duration: 250},
      {id: 9, title: "task09", duration: 119},
      {id: 10, title: "task10", duration: 560},
      {id: 11, title: "task11", duration: 340},
      {id: 12, title: "task12", duration: 45},
      {id: 13, title: "task13", duration: 86},
      {id: 14, title: "task14", duration: 480},
      {id: 15, title: "task15", duration: 900}
];
```

4. Count total hours worked in 1 day ? // => 39
```javascript=
/**
 * Count total hours worked in 1 day for all employees.
 * @return {number} total hours worked in a day
 */
function totalHoursWorkedOneDay() {
    let totalHrs = 0 ;
    for (const emp of employees) {
        let tp = employeeType[ emp.type-1] ;
        
        let hrS= parseInt( tp.work_begin.substring(0,2) );
        let hrE = parseInt( tp.work_end.substring(0,2) ) ;
        if( hrE===0 ){
           hrE = 24;
        }

        let minS= parseInt( tp.work_begin.substring(3,5) );
        let minE = parseInt( tp.work_end.substring(3,5) ) ;

        let secS= parseInt( tp.work_begin.substring(6,8) );
        let secE = parseInt( tp.work_end.substring(6,8) ) ;

        let startTime = hrS*60*60 + minS*60 + secS;
        let endTime = hrE*60*60 + minE*60 + secE;

        let diffHrs =( endTime - startTime )/60/60;
        totalHrs = totalHrs + diffHrs ;
    }
    return totalHrs;
}
```

5. Make a function that take as parameters dayTime and return number of employee working // howManyEmployeeByTime(time) => int

```javascript=
/**
 * Count number of employees at given time.
 * @param {string} time in format 00:00:00
 * @return {number} number of employees at that time
 */
function howManyEmployeeByTime( time ) {

    // check input
    if( time.match('[0-9][0-9]:[0-9][0-9]:[0-9][0-9]') === null){
        throw 'Time Format is unacceptable. Format is "00:00:00"';
    }

    let count = 0;
    for (const emp of employees) {
       let tp = employeeType[ emp.type-1] ;
       let start = tp.work_begin;
       let end = tp.work_end;
       if(end === "00:00:00"){
           end = "24:00:00";
       }
       if(time >= start && time <=end){
           count++;
       }
    }
    return count;
    
}

// test 
howManyEmployeeByTime( '15:01:01') ; // 4
```

6. How many days of work needed to done all tasks ? // => 1 day = 9:00 to 00:00 between 00:00 and 09:00 doesnt count.

```javascript=
/**
 * Count number of days of work needed to complete all tasks.
 * @return {number} number of days of work to complete all tasks
 */
function howManyDaysToCompleteAllTasks() {
    let totalMin = 0;
    let workHrsDay = 42;
    for (const it of tasks) {
        totalMin += it.duration;
    }
    let totalHrs = totalMin / 60;
    let result = Math.ceil(totalHrs / workHrsDay);
    return result;
}
```