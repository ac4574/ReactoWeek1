# Array Three Sum
## Interviewer Prompt
Given a sorted array of distinct integers and an integer representing a target sum, write a function that returns an array of all triplets in the input array that sum to the target sum.


### Examples

```javascript
arrayThreeSum([-8, -6, 1, 2, 3, 5, 6, 12], 0)   //should return [[-8, 2, 6], [-8, 3, 5], [-6, 1, 5]]
arrayThreeSum([-9, 1, 2, 3, 5, 6, 7], 35)    //should return []
arrayThreeSum([-5, -3, 1, 2, 6, 12, 15], 10)  //should return [[ -3, 1, 12 ]]
```

## Interviewer Strategy Guide
Let's think about time complexity.  If your interviewee tries to go down the path of trying all possible combintations of three integers, that will have a time complexity of O(n^3).  Try to encourage them to find a more optimal approach.

### Hints
- Suggest to the interviewee to think of how they would handle optimizing this problem if they only had to sum two integers.  A three sum can be thought of as trying to find if two integers sum to target valued added to each integer in the input array.

For example:
```javascript
arrayThreeSum([11, -1, -10], 0) 
//can be broken down to the below:

//checking if 11 is going to be one of the integers: 11 + ? + ? = 0
arrayTwoSum([-1,-10], 11 + 0)

//checking if -1 is going to be one of the integers: -1 + ? + ? = 0
arrayTwoSum([11,-10], -1 + 0)

//checking if -10 is going to be one of the integers: -10 + ? + ? =0
arrayTwoSum([11,-1], -10 + 0)
```

## Solutions

**Naive Solution**: O(n^3) time complexity (yikes!), O(n) space complexity
```javascript
function arrayThreeSum(array, target) {
  const solution = []

  for (let i = 0; i < array.length - 2 ; i++) {
    let first = array[i]
    for (let j = i+1; j < array.length - 1 ; j++) {
      let second = array[j]
      for (let k = j+1; k < array.length ; k++) {
        let third = array[k]
        if (first + second + third === target) {
          solution.push([first, second, third])
        }
     }
    }
  }
  return solution
}
```

**Optimized Solution 1**: O(n^2) time complexity, O(n) space complexity
```javascript
function arrayThreeSum(arr, targetSum){

  const solution = []

  for (let i = 0; i < arr.length-2; i++){
    let element = arr[i]
    let leftIndex = i + 1
    let rightIndex = arr.length - 1

    //for each element in the array check to see if any two other integers in the array add to the target sum
    while(leftIndex < rightIndex){
      let currentSum = element + arr[leftIndex] + arr[rightIndex]

      //if the currentSum is equal to the target sum add an array of those 3 integers to the solution array
      if(currentSum === targetSum){
        solution.push([element, arr[leftIndex], arr[rightIndex]])
        leftIndex ++
        rightIndex --
      } else if(currentSum > targetSum){
        rightIndex --
      } else if (currentSum < targetSum){
        leftIndex ++
      }
    }
  }
  return solution
}
```


**Optimized Solution 2**: O(n^2) time complexity, O(n) space complexity
- Note that this solution does not return a sorted solution like Solution 1

```javascript
function arrayThreeSum(arr, targetSum){
  const solution = []
  for(let i = 0; i< arr.length-1; i++){
    //the sum needed given we already know one element arr[i]
    let currentSum = targetSum - arr[i]
    //create a hash table to store all of the integers from arr[i] we have tried
    let memo ={}
    for (let j = i+1; j < arr.length; j++){
      if(memo[currentSum-arr[j]]){
        solution.push([arr[i], currentSum-arr[j], arr[j]])
      }else{
        memo[arr[j]] = true
      }
    }
  }
  return solution
}
```
