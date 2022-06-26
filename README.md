# SolidityNotes - 0.8

## DATA TYPES

Like other languages solidity also support data types to be defined with values and references

### DATA TYPES DEFINED USING VALUES

```solidity
bool public b = true;       // true | false
uint public u = 123;        // unsigned integers only store positive numbers
                            // uint = uint256 0 to 2^256-1
                            //        uint8   0 to 2^8-1
int public i = -123;        // int = int256     -2**255 to 2**255-1
                            //       int128     -2**127 to 2**127-1
int public minInt = type(int).min;      // gives you minimum value for int
int public maxInt = type(int).max;      // gives you maximum value for int

addresss public a = 0x539148A4945ff39596751927ebB6B428bb2ADcE2;         // address type stores a address hash
bytes32 public b32              // stores bytes32 address
```

## FUNCTIONS IN SOLIDITY

While defining functions we need to explicity define some things related to it. Just get the hang of the syntax we will see what all this different keywords mean.

```solidity
function add(uint x, uint y) external pure returns (uint) {
    return x + y;
}

function sub(uint x, uint y) external pure returns (uint) {
    return x - y;
}
```

## VARIABLES IN SOLIDITY

### STATE VARIABLES

These are the variables which actually store data on blockchain. The variables defined outside any function directly inside the contract are state variables.

### LOCAL VARIABLES

Variables inside a function are local variables and are available only while the function is executing

```solidity
contract StateVariables {
    uint public u = 123;          // data stored in this variable will be stored on the blockchain and hence it is a state variable

    function foo() external {
        uint v = 456;             // local variable
    }
}
```

Value defined with u will be available after the contract is deployed and is always available but that's not the case with local variables

### GLOBAL VARIABLES

Variables that are available globally

```solidity
contract GlobalVariables {
    function globalVars() external view returns () {
        address sender =  msg.sender;         // this variables stores the address that called this function
        uint timestamp = block.timestamp;     // this stores the unix timestamp of when this function is called
        uint blockNum = block.number;         // this stores the value of current block number
    }
}
```

## VIEW AND PURE FUNCTIONS

View functions can read data from blockchain while pure cannot.

```solidity
contract ViewAndPureFuntions {
    uint public num;

    function viewFunc() external view returns (uint) {
        return num;             // since it is using num we define it as view
    }

    function pureFunc() external pure returns (uint) {
        return 1;               // this does not modify anything on the blockchain and is a readonly function
    }
}
```

## COUNTER IN SOLIDITY

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Counter {
    uint public count;

    function inc() external {
        count += 1;
    }

    function decc() external {
        count -= 1;
    }
}
```

## DEFAULT VALUES OF STATE VARIABLES

```solidity
contract DefaultValues {
    bool public b;          // false
    uint public u;          // 0
    int public i;           // 0
    address public a;       // 0x0000000000000000000000000000000000000000 - a seqence of 40 zeros
    bytes32 public b32;     // 0x0000000000000000000000000000000000000000000000000000000000000000 - a seqence of 64 zeros
}
```

## CONSTANTS IN SOLIDITY

If state variables does not change you can define it as constant. Also defining them as constants result in lower gas consumption when calling or excessing them.

`address public constant MY_ADDRESS = <address>;`

## IF-ELSE STATEMENTS IN SOLIDITY

```solidity
contract ifElse {
    function example(uint x) external pure returns (uint) {
        if (x<10) {
            return 1;
        } else if (x < 20) {
            return 2;
        } else {
            return 3;
        }
    }

    function ternary(uint x) external pure returns (uint) {
        return x < 10 ? 1 : 2;
    }
}
```

## FOR AND WHILE LOOPS

```solidity
contract forAndWhileLoops {
    function loops() external pure {
        for (uint i=0; i < 10; i++) {

            // code

            if ( i==3 ) {
                continue;       // more code will not be executed for this iteration
            }

            // more code

            if ( i==5 ) {
                break;          // loop will break
            }
        }

        uint j = 0;
        while(j<10) {
            j++;
        }
    }

    function sum(int n) external pure returns (uint) {
        uint sum;
        for(uint i=0; i<n;i++) {
            sum += i;
        }
        return sum;
    }
}
```

keep loops small in solidity because they consume gas

## ERRORS IN SOLIDITY

There are 3 ways to throw error require, revert, assert and when an error occurs gas will be refunded and variable updations will be reverted

```solidity
contract Error {
    function testRequire(uint _i) public pure {

        require(_1<10, "i>10");
    }

    function testRevert(uint _i) public pure {

        if(_i > 10) {
            revert("i>10");
        }
    }

    function testRevert(uint _i) public pure {

        if(_i > 10) {
            revert("i>10");
        }
    }

    error MyError(address caller);

    function customError(uint _i) public pure {

        if(_i > 10) {
            revert(MyError(msg.sender, _i));
        }
    }
}

```

assert is used to check for conditions that should always be equal to true

## FUNCTION MODIFIERS

reuse code before and/or after function

```solidity
modifier lessThan10(uint _x) {
    require(_x < 10_, "paused");
    _;
}

function inc(uint _x) external whenNotPassed(_x) {
    count += 1;
}

function dec(uint _x) external whenNotPassed(_x) {
    count -= 1;
}
```

## CONSTRUCTOR

Functions that are only called for the first when the contract is deployed

```solidity
contract Constructor {
    address public owner;
    uint public x;

    constructor(uint _x) {
        owner = msg.sender;         // we can this way set the owner of the contract
        x = _x;
    }
}
```

## A SMALL SIMPLE CONTRACT

```solidity
contract Ownable {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "not owner");
        _;
    }

    function setOwner(address _newOwner) external onlyOwner {
        require(_newOwner != address(0), "invalid address");
        owner = _newOwner;
    }

    function onlyOwnerCanCallThisFunc() onlyOwner{
        // code
    }

    function anyOneCanCallThisFunc() {

    }
}
```

## FUNCTION OUTPUTS

Functions in solidity can return multiple outputs and those can be named outputs as well. You can also destructure the outputs

```solidity
contract FunctionOutputs {
    function returnMany() public pure returns (uint, bool) {
        return (1, true);
    }

    function namedOutputs() public pure returns (uint x, bool b) {
        return (1, true);
    }

    function assignedOutputs() public pure returns (uint x, bool b) {
        x = 1;
        b = true;
    }
    In this function you don't have to explicity return the values, you just set the named outputs to those values

    function destructuingAssignment() public pure {
        (uint x, bool b) = returnMany();        // destructure both variables
        (, bool b) = returnMany();        // destructure one variable, you still have to put a comma
    }
}
```

## ARRAYS IN SOLIDITY

```solidity
contract Array {
    uint[] public nums = [1, 2, 3];         // dynamic array
    uint[3] public numsFixed = [4, 5, 6];   // fixed array of 3 elements, if you have more than 3 elements the contract will not compile

    function examples() external {
        nums.push(4);       // nums = [1,2,3,4]   you can not push to fixed size array
        uint x = nums[0];   // gives first element of array
        nums[2] = 77;       // nums = [1,2,77,4]   reassign the element at index 2
        delete nums[1];     // nums = [1,0,77,4]   replaces the element with 0
        nums.pop()          // removes the last element from array and shrinks the size
    }

    // create array in memory
    uint[] memory a = new uint[](5);        // a in memory array should be of fixed size
    // you can use push, pop with in memory array, u can just get and update the value

    // return an array
    function returnArray() external view returns (uint[] memory) {      // memory tells that first copy the nums in memory and return it
        return nums;
    }
}
```

Although u can return array from a function but it is not recommended because of the amount of gas it will use. If the array is large it can use up all of the gas

Remove elements from array  
https://www.youtube.com/watch?v=szv2zJcy_Xs&list=PLO5VPQH6OWdVQwpQfw9rZ67O6Pjfo6q-p&index=20
https://www.youtube.com/watch?v=8i4CChP99XQ&list=PLO5VPQH6OWdVQwpQfw9rZ67O6Pjfo6q-p&index=21

## MAPPING

It is like hashmap / dictionary from other languages

```solidity
contract Mapping {
    mapping(address => uint) public balances;
    mapping(address => mapping(address => boolean)) public isFriend;

    function examples() external {
        balances[msg.sender] = 123;         // setting the value
        uint bal = balances[msg.sender];    // getting the value

        uint bal2 = balances[address(1)];   // although we have not the balance for this address it will return us 0 which is the default value

        balances[msg.sender] = 456          // updates the balance
        balances[msg.sender] += 789         // increase the balance

        delete balances[msg.sender];        // value will be reset to 0

        isFriend[msg.sender][address(this)] = true;
    }
}
```
