# Wasm Protected Site 2(Web) - Writeup

## description

````
By Andrew

Similar to wasm protected site 1, but this time there is no password, only the flag.
Enter the flag, and the program will check it for you
http://web.bcactf.com:49158/

Hint 1 of 1
What does the wasm do to compare each byte
````


## answer

As in Wasm Protected Site 1, if you check main.js, the check process will be downloaded in /code.wasm.
This time we need to check the flag itself.

The contents of the WebAssenbly are as follows.
It would be nice if we could process the flag somehow to become `bjsxPKMH|\227N\1bD\043b]PR\19e%\7f/;\17`.


````
(module
  (memory $memory (;0;) (export "memory") 1)
  (func $cmp (;0;) (param $v0 (;0;) i32) (param $v1 (;1;) i32) (result i32)
    (local $v2 (;2;) i32)
    loop $label0
      local.get $v2
      local.get $v0
      i32.add
      i32.load8_u
      local.get $v2
      local.get $v1
      i32.add
      i32.load8_u
      local.get $v2
      i32.const 9
      i32.mul
      i32.const 127
      i32.and
      i32.xor
      i32.ne
      local.get $v2
      i32.const 27
      i32.ne
      i32.and
      if
        i32.const 0
        return
      end
      local.get $v2
      i32.const 1
      i32.add
      local.tee $v2
      i32.const 1
      i32.sub
      local.get $v0
      i32.add
      i32.load8_u
      i32.eqz
      if
        i32.const 1
        return
      end
      br $label0
    end $label0
    i32.const 0
    return
  )
  (func $checkFlag (;1;) (export "checkFlag") (param $a (;0;) i32) (result i32)
    local.get $a
    i32.const 1000
    call $cmp
    return
  )
  (data (i32.const 1000) "bjsxPKMH|\227N\1bD\043b]PR\19e%\7f/;\17")
)
````

If you look at the process of wasm, you can see that 0x05b compares values.
At 0x06f, it increments to the next character = the current number of characters.
In other words, if we check the value of stack at this time, we can find the flag string one character at a time.

For example, for the first character, the value is compared with the 98 decimal character code of b, and then the counter in stack0 is incremented by 1.


###### bcactf{w4sm-w1z4rDry-Xc0wZ}
