# The `Of` Programming Language


`Of` is yet another functional programming language, which is a dialect of `Lisp` and is inspired by `Go`.


- All expressions in `Of` are in the `Subject-Predicate` structure.
- Results of expressions are nil, vars or values, but not actions.
- Vars and values can be operated by operators, but actions cannot.


```
	# sequence
	((a f) b g)  # f g h are functions
	((a f) (b b g) c d h)
```


```
	# branch
	((a 3 %)
		(0 ("=0" Print))
		(1 ("+1" Print))
		(2 ("-1" Print))
	Case
	)


	((a 3 >) (True (">3" Print)) Case)
```


```
	# loop
	( (10 20 ..) i x (i x Println) Loop)
	# 0 10
	# 1 11
	# ...


	( (10 20 ..) i _ (i (10 i *) Println) Loop)
	# 0 0
	# 1 10
	# ...


	( (10 20 ..) _ x (x (10 x *) Println) Loop)
	# 10 100
	# 11 110
	# ...
```


```
	# function
	(fib (n Int) Int
		((n 1 >) (True (((n 2 -) fib) ((n 1 -) fib) +)) (False n) Case)
		Func)
	((8 fib) Print)
	# 21


	(min ((x Int) (y Int)) Int
		((x y >) (True y) (False x) Case)
		Func)
	((5 8 min) Print)
	# 5
```


```
	# conio
	(a b c Int)

	(a Scan)
	(b c Scan)

	(a b Println)
	("c is " c Print)
```


```
	# file
	(
		(fname1 fname2 String)
		(f1 f2 File)
		(a Int)

		(fname1 "a.txt" Assign)
		(fname2 "~/b.dat" Assign)

		(f1 (fname1 Open) Assign)
		(f2 (fname2 "bin-creat_read_write" Open) Assign)

		# read/write all files/devs
		(f1 a Read)
		(f2 a Write)

		(f1 Close)
		(f2 Close)
	)
```


```
	( myPkg (
		(math _ Import)
		(format "fmt" Import)
		# ...
	) Pack)
```


```
	# arithmetic
	(a b +)
	(a b -)
	(a b *)
	(a b /)
	(a b %)
	(a b ^) # exp
	(a b +=)
	(a b -=)
	(a b *=)
	(a b /=)
	(a b %=)
	(a b ^=)
	(a ++)
	(a --)
	(a +-) # negative

	# comparing
	(a b >)
	(a b <)
	(a b >=)
	(a b <=)
	(a b ==) # True or False
	(a b !=) # True or False
	(a b <>) # 1 0 or -1

	# logic
	(a !)
	(a b &&)
	(a b ||)
	(a b $$) # xor
	(a b !$) # xnor

	(3 3 ==)
	(Int Int ==)
	(+ + ==)
```


```
	# types & constants
	# Char Int Float Bool String File

	(name Type String)
	(aaa "Tom" name)
	(aaa TypeIs) # name

	# special consts
	True
	False

	Im
	Re
	Nil
	Inf
```


```
	# list
	(1 4 ..) # (1 2 3)
	(1 4 :) # ((1) (2) (3))
	(5 Zeros) # (0 0 0 0 0)
	(5 Ones) # (1 1 1 1 1)
	(5 Randoms) # (0.72544713 0.23140609  0.64072281  0.27792029 0.0219964)

	(als `(1 2 3 4 5 6 7 8 9) Assign)
	(als 0 @) # 1
	(als -1 @) # 9
	(als `(2 4) @) # (3 5)
	(als (2 4 ..) @) # (3 4 5)
	(als First) # 1
	(als Last) # 9
	(als NoFirst) # (2 3 4 5 6 7 8 9)
	(als NoLast) # (1 2 3 4 5 6 7 8)

	(als (als 0 Append) Assign) # als=(1 2 3 4 5 6 7 8 9 0).  als is NOT changed by append itself.
	(als (als 12 Prepend) Assign) # als=(12 1 2 3 4 5 6 7 8 9 0)
	(als 101 Push) #  als=(12 1 2 3 4 5 6 7 8 9 0 101)
	(als 102 Enqueue) #  als=(12 1 2 3 4 5 6 7 8 9 0 101 102)
	(als x Pop) # pop 1 item. x=102, als=(12 1 2 3 4 5 6 7 8 9 0 101)
	(als y Dequeue) # dequeue 1 item. y=12, als=(1 2 3 4 5 6 7 8 9 0 101)
	(als (als 2 0 Insert) Assign) # als=(99 100 0 1 11 12 2 3 4 5 6 7 8 9 0 101 9 8 7)
	(als (als 2 x Remove) Assign) # x=2, als=(99 100 1 11 12 2 3 4 5 6 7 8 9 0 101 9 8 7)

	(als Length) # 13, get length
	(als (als Reverse) Assign) # als=(102 101 100 9 8 7 6 5 4 3 2 1 103)
	(als (als Shuffle) Assign) # als=(3 101 7 1 6 2 103 8 4 102 100 5 9))
	(als (als `(9 10 11 12 0 1 2 3 3 4 5 6 7 8) SortBy) Assign) # als=(100 101 102 103 1 2 3 4 5 6 7 8 9)

	(`(121 34 56 33) Keyed) # ((0 121) (1 34) (2 56) (3 33))
	(`(1 4 3 4 1 9 9 2) Unique) # (1 4 3 9 2)

	(`((a b) (c (d) e) (f (g h))) 1 Flat) # (a b c (d) e f (g h))
	(`(100 101 102 103 1 2 4) `(4 1) Partition) # (100 101 102 103) (101 102 103 1) (102 103 1 2) (103 1 2 4)

	(`((1 2) (3 4) (5 6)) Transpose) # ((1 2 5) (2 4 6))
	(`(0 0 1 0 2 1 5) Tally) # ((0 3) (1 2) (2 1) (5 1))

	Map
	Reduce
	Filter
```
