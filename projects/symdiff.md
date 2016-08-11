---
layout: post
title: Symdiff
permalink: /projects/symdiff/
categories: project
---

[Symdiff][1] is a symbolic differentiation library. I made it to better understand 
how Theano and Tensorflow work internally. I made several implementations 
in different languages, including C++, C and python. I also added LLVM-IR codegen
as I have always been interested in using LLVM in one of my projects. 

Why have I implemented the same thing 3 times? It was easier to start out 
with a garbage collected language (python). Later, I wanted to
 see how I could do it in C++ without losing too much expressivity (the C++
version is more complicated but the public API is almost as simple
 as python's). Finally, since Object Oriented programming is a wonderful tool to build graphs with, I wanted to 
try to build my own OOP system in C with virtual calls (inheritance is also
supported in a way).


# Python

    x = Unknown('x')
    y = Unknown('y')

    env = {x: scalar(5), y: scalar(2)}

    f = x ** 3 - y ** 2   
	print(f)
	
    dfdx = f.derivate(x)
	dfdy = f.derivate(y)
	
	val = f.eval(env)
	
# C++

	/*  Expr building */
	Sym x = make_var("x");
	Sym y = make_var("y");

	Sym f = x * x * y;

	f.print(std::cout) << std::endl;

	Sym df = f.derivate("x");

	df.print(std::cout) << std::endl;

	/*  Full Eval */
	Context env = { {"x", make_val(4)}, {"y", make_val(3)} };

	std::cout << f.full_eval(env) << std::endl;
	
# C

	SymExpr x = sym_placeholder("x");
	SymExpr expr = sym_mult(x, x);

	sym_print(expr);	 printf("\n");

	SymExpr df = sym_deriv("x", expr);

	sym_print(df);	 	 printf("\n");

	sym_free(df);
	sym_free(expr);
	sym_free(x);


[1]: https://github.com/Delaunay/symdiff
