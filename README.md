HAssembly-evm
=============

## EVM (Ethereum virtual machine) Assembly on Haskell DSL

This is a bytecode generator from EVM assembly on Haskell DSL.

Warning:
  * A big **WIP**
  * No guarantee and under testing
  * Experimental and conceptual project

Feature:
  * Composable assembly
  * Purely functional implementation
  * Simple and slow implementation for readability
  * Only a few dependent libraries and GHC extensions

Limitation:
  * Weak error detection
  * Jump size is limited to 2 bytes


## Run

Command:

  * `$ runghc ...`
  * `$ stack ...`
  * `$ cabal ...`

Example:

  ```sh
  $ runghc xxx
  60606040523415600e5760008...
  ```


## Code example

Basic:

  ```Haskell
  main :: IO ()
  main = putStrLn $ codegen prog1
  
  prog1 :: EvmAsm
  prog1 = do
      push1 0x10
      push1 0x20
      add
  ```

Composable:

  ```Haskell
  prog2 :: EvmAsm
  prog2 = do
      prog1       -- compose
      push1 0x5
      sub
  ```

Pseudo instruction and built-in function:

  * `_dest` and `_label` : pseudo instruction for jump and push with symbol
  * `_raw` : pseudo instruction for raw byte
  * `_codeSize` : built-in for code siez

  ```Haskell
  prog3 :: EvmAsm
  prog3 = do
      push1 0x60
      push1 0x40
      mstore
      _jump "target2"
  
      push1 (_codeSize prog4)
      _push "top1"
      _dest "target2"
  
  
  prog4 :: EvmAsm
  prog4 = do
      _label "top1"
      _raw 0x7
      _raw 0x8
  ```

Haskell host language:  
Example1:

  ```Haskell
  prog5 = do
      if isBizantinum          -- if expression on Haskell
          then prog_header1
          else prog_header2
      push1 0x60
      push1 0x40
      mstore
  ```

Example2:

  ```Haskell
  shiftL nbit = do    -- user defined function
      push1 nbit
      push1 2
      exp
      mul
  
  prog6 = do
      mload
      shiftL 16    -- using
      push1 0x1
      add
  ```


## Execute bytecode

If you need to execute a generated bytecode, please use the [`evm` command](https://github.com/ethereum/go-ethereum) of go-ethereum project or some tools.


## See also
  * [Ethereum Yellow Paper](https://github.com/ethereum/yellowpaper)
  * [Go Ethereum](https://github.com/ethereum/go-ethereum)
  * [The Solidity Contract-Oriented Programming Language](https://github.com/ethereum/solidity)
  * [Ethereum EVM illustrated](https://github.com/takenobu-hs/ethereum-evm-illustrated)
