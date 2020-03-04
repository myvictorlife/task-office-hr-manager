var Mocha = require('mocha')
var mocha = new Mocha()
mocha.suite.emit('pre-require', this, 'solution', mocha)

var chai = require('chai')
var { assert, expect } = chai
var sinon = require('sinon')
var sinonChai = require('sinon-chai')

chai.should()
chai.use(sinonChai)

/*#######################################
  DO NOT EDIT ABOVE THIS LINE
#######################################*/

/****************************************

  Question 1
  - Debouncing
  
  There are some browser events that can fire many times within a 
  short timespan, such as resizing a  window or 
  scrolling down a page. When an event like this takes a long 
  time to execute, it will eventually cause a slowdown 
  and choppy rendering. The solution to this problem is called
  debouncing. 
  
  Create a function that will take a function, and a delay 
  (in milliseconds) as parameters, and return a new function. 
  This function should reset the timeout everytime it is called, 
  and prevent the passed in function from executing until the 
  timeout has expired.
  
  Run 'debounceSimulator' when you are ready. It will call try to
  call the debounced scroll function 20 times. 'scroll' should 
  only be called once.
  
  
****************************************/

function debounce(fn, delay) {
  let timeout;
  
  return function() {
    clearTimeout(timeout)
    timeout = setTimeout(fn, delay);
  }
}


// Uncomment next line when ready to run
runDebounceSimulator()

/****************************************

  Question 2A
  - Odd Loop Behavior
  
  Execute the function below, 'runOddLoopBehavior' and evaluated the output.

  Write your answer below, why you believe this is happening.
  
  -----
  
  Var and Let are both used for variable declaration in Javascript, but in this case We have a problem. 
  This is because the variable 'i' is declared globally using 'var' to solve this it must use 'let' for 
  variables to be declared within the scope (block scoped).
 
  -----

  Question 2B
  
  Without changing any code within the setTimeout function, including the 
  setTimeout itself, make this loop print out the correct index values.
  
  * Hint: Do you know how js scope / closures work? How can you take        
  * advantage of those to solve this problem 
  
  * Note: The 'indexes' array is just for testing for correct answer.
  
****************************************/

const numbers = [6, 3, 2, 1, 5, 7, 9, 8, 5, 1];
function loopOverNumbers(){
  const indexes = []
  for (let i = 0; i < numbers.length; i++) {
    // DO NOT MODIFY SET TIMEOUT
    setTimeout(function() {
      console.log('The index of this number is: ' + i);
      indexes.push(i)
    }, 1000)
    // DO NOT MODIFY SET TIMEOUT

  }
  return indexes
}

// Uncomment next line when ready to run
runOddLoopBehavior()


/****************************************

  Question 3
  - Below is a slightly outdated way of implementing a 'wait' function. It was very common to use synchronous
  functions with callbacks in the past. But recently, Promises have become a better way to accomplish this.
  
  - Convert the below code into a Promise. When ready, uncomment 'runConvertToPromise'
    
  Note: Should be usable by using 'Promise.then' or 'async await' syntax
  
    wait(100).then(() => callback())
  
      OR
  
    async () => {
      await wait(100)
      callback()
    }
  
  
****************************************/

// Note: Function name must be 'wait'
async function wait(ms, callback){
  setTimeout(() => {
    if(callback){
      callback
    }
  }, ms)
}

runConvertToPromise()

/****************************************

  Question 4
  - Prime Factorization
  
  Write a function that returns the prime factorization of the given number.
  
  Calculating a prime factorization is finding the smallest prime numbers that multiply
  together to make the original number.
    
  eg: Prime factors for 
    4 = 2 * 2
    18 = 2 * 3 * 3
    50 = 2 * 5 * 5
  
****************************************/

// Note: Function name must be 'findPrimeFactors'
function findPrimeFactors(n) {
  let primeArray = []
  
  while (n % 2 === 0) {
      primeArray.push(2);
      n = n / 2;
  }
  
  let sqrtNum = Math.sqrt(n);
  for (let i = 3; i <= sqrtNum; i++) {
      while (n % i === 0) {
          primeArray.push(i);
          n = n / i;
      }
  }
  
  if (n > 2) {
      primeArray.push(n);
  }
  
  return primeArray
}

// Uncomment next line when ready to run
runFindPrimeFactors()

/*#######################################
  DO NOT EDIT BELOW THIS LINE
#######################################*/

const spiedScroll = sinon.spy(scroll)
function scroll(){
  // console.log('Scrolling')
}


function runDebounceSimulator(){
  let iterations = 20
  describe(`Question 1 - Debouncing`, function(){
    it(`The 'scroll' function should have been called once`,function(done){
      let debouncedScroll = debounce(spiedScroll, 200)
      for(let i = 0; i < iterations; i++){
        debouncedScroll()
      }
      setTimeout(() => {
        expect(spiedScroll.callCount).to.equal(1)
        done()
      }, 600)
    })
  })
}

function runOddLoopBehavior(){
  const expectedIndexes = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
  describe(`Question 2 - Odd Loop Behavior`, () => {
    it(`Should return an array of the numbers 0 -> 9`, (done) => {
      let indexes = loopOverNumbers()
      setTimeout(() => {
        expect(indexes).to.deep.equal(expectedIndexes)
        done()
      }, 1500)
    })
  })
}

function runConvertToPromise(){
  const callback = () => console.log('Timer has ended!')
  
  describe(`Question 3 - Convert to Promise`, () => {
    it(`Should return an instance of promise`, () => {
        expect(wait(10) instanceof Promise).to.be.true
    })
    describe(`Should wait the correct amount of time (May fail on certain browsers)`, () => {
      it(`100ms`, (done) => {
          let fail = setTimeout(() => {
             throw new Error('Waited to long to resolve')
          }, 120)
          wait(100).then(() => {
            clearTimeout(fail)
            done()
          })
          
      })
      it(`250ms`, (done) => {
          let fail = setTimeout(() => {
             throw new Error('Waited to long to resolve')
          }, 300)
          wait(250).then(() => {
            clearTimeout(fail)
            done()
          })
      })
      it(`1000ms`, (done) => {
          let fail = setTimeout(() => {
             throw new Error('Waited to long to resolve')
          }, 1100)
          wait(1000).then(() => {
            clearTimeout(fail)
            done()
          })
      })
    })
  }) 
}

function runFindPrimeFactors(){
  const primeFactorsList = [4, 50, 616, 4000, 12525, 81169]
  const expectedList = [ 
    [ 2, 2 ],
    [ 2, 5, 5 ],
    [ 2, 2, 2, 7, 11 ],
    [ 2, 2, 2, 2, 2, 5, 5, 5 ],
    [ 3, 5, 5, 167 ],
    [ 11, 47, 157 ] 
  ]
  const actualList = []
  
  describe(`Question 4 - Prime Factorization`, () => {
    for(var i = 0; i < primeFactorsList.length; i++){
      let integerToTest = primeFactorsList[i]
      let actual = findPrimeFactors(integerToTest)
      let expected = expectedList[i]

      it(`Prime factors of ${integerToTest} should be ${expected}.`, () => {
        expect(actual).to.deep.equals(expected)
      })

      actualList.push(actual)
    }
  }) 
}

mocha.run()