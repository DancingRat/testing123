# Jest(creating test case) Setup
## Installation of Jest
```
yarn add --dev jest

if you're using typescript, you also have to,
yarn add --dev typescript ts-jest @types/jest @types/node ts-node ts-node-dev
```

## Create and configure Jest config file
1. Create a config file
```
yarn ts-jest config:init
```
## Create a jest test file
- the format should be <u>filename</u>.test.ts
```javascript
you can create a dummy test file to test out
e.g.
I am gonna create a test file call dummy.test.ts; like below

it("dummy",()=>{
    expect(1 + 1).toBe(2)
})
```
## Running Jest test cases
- run all test cases
```
yarn jest
```