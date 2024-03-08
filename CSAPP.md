# CH 1 A tour of Computer Systems

## Concurrence

Instructions are interleaved with the instructions of another process.

## Context Switch

Interleaving is implemented by *context switch*, 

1. save the context of the current process
2. restore the context of the new process
3. pass control to new process

![context switch](https://s2.loli.net/2024/02/26/xGU2yPcKoHijkBE.png)

## Network

Network can be viewed as just another I/O device.

# CH 2 Representing and Manipulating Information

## 1 byte = 8 bit

Why 8?

Original ASCII is 7 bit.

> You can get a ascii table by typing `ascii man` command.

![ascii table](https://s2.loli.net/2024/02/26/pt1wa3TCoIAVmv8.png)

## Little / Big Endian

LSB(Least Significant Bit) / MSB(Most Significant Bit) comes first.

## Boolean Algebra

### bit-level operator

~(NOT), &(AND), |(OR), ^(EXCLUSIVE-OR)

![bit operator](https://s2.loli.net/2024/02/26/myHurYMZxkJsPSU.png)

## Shift Operation

### Left Shift

### Right Shift

1. logical(for unsigned integer )

   fill the left end with k zeros, giving a result $[0,...,0,x_{\omega-1},x_{\omega-2}，...,x_k]$

2. arithmetic(for signed integer)

   fill the left end with k MSB, giving a result $[x_{\omega-1},...,x_{\omega-1},x_{\omega-1},x_{\omega-2}，...,x_k]$

## Expanding the Bit Representation of a Number

1. zero extension

   for unsigned integer

2. sign extension

   for signed integer

ii