# Promises

A `promise` is in one of these states:

- **pending**: neither fulfilled nor rejected
- **fulfilled**: meaning the operation was completed successfully
- **rejected**: meaning the operation failed

***A promise is an object***

```jsx
const promise1 = new Promise(function (resolve, reject) {
    setTimeout(function () {
        console.log("task has been completed");
        resolve()
    }, 2000)
})

promise1.then(function () {
    console.log("promise is consumed")
})

new Promise(function (resolve, reject) {
    setTimeout(function () {
        console.log("asnyc task 2");
        resolve();
    }, 2000)

}).then(function () {
    console.log("Async task 2 completed")
})

const promise3 = new Promise(function (resolve, reject) {
    setTimeout(function () {
        resolve(
            {
                username: "ashCode98",
                password: "pwd"
            }
        )
    }, 2000)
})

promise3.then(function (username) {
    console.log(username)
})

const promise4 = new Promise(function (resolve, reject) {
    setTimeout(function () {
        let error = false;
        if (!error) {
            resolve({
                username: "passing",
                email: "passing@gmail.com"
            })
        }
        else {
            reject('ERROR: something is wrong!!')
        }
    }, 1000);
})
promise4.then((user) => {
    // console.log(user.email)
    return user.email
}).then((email) => {  //it is called chaining
    console.log(email)
}).catch((e) => {
    console.log(e)
})

```