---
layout: post
title:  "Why Julia"
date:   2021-06-11 09:10:00 -0400
categories: physics jupyter Julia Python
---
Although we currently have been using python to teach our majors computing, the problem set questions I am developing will have composed with both Python and Julia versions. I've been keeping my eye on Julia's development over recent years, and I feel that it has now reached a stage where it is more than sufficient for most of what I need to do in terms of teaching undergraduate physics, and sufficient to for data analysis in the laboratory. 

So why introduce Julia into the mix? I've been using Python for many years, and am mostly happy with it...until I have to do a computation that is not so trivial. Julia's syntax is very similar to Python, and even has features that I appreciate as a physicist:

* simple functions can be defined in one line such as f(x) = x^2
* Julia understands that 2x = 2*x
* array indexing starts at 1 (I know this is personal, but I *really* like this feature)
* arrays are built in and *fast* without having to import something like NumPy
* Julia is *very* fast

The speed is impressive. For instance, here is some code to compute the magnetic field on the axis of a current loop:

{% highlight julia %}
using LinearAlgebra
using Plots
using LaTeXStrings
"""
    b_field(I, R, z, δϕ)

Computes the on-axis magnetic field in nanoTesla for a current 
loop with radius `R` and carrying current `I`

...
# Arguments
- `R::Float64` : radius of loop in meters; wire radius assumed negligible
- `I::Float64` : current in loop in Amperes; + current ccw when viewed from +z
- `z::Float64` : on-axis distance from plane of loop in meters
- `δϕ::Float64`: angular step in radians (used to integrate around loop)
...

# Examples
```julia-repl
julia> b_field(1.0, 1.0, 0.0, 0.0001)
3-element Vector{Float64}:
   0.0
   0.0
 628.31
```
```julia-repl
julia> b_field(1.0,1.0,1.0,0.0001)
3-element Vector{Float64}:
  -0.00301606
   2.7945e-7
 222.141
```
"""
function b_field(I, R, z, δϕ)
    B = [0.0, 0.0, 0.0]
    factor = 1.0e-7
    
    for ϕ in 0.0:δϕ:2π-δϕ
        Idl = [-sin(ϕ), cos(ϕ), 0.0 ]*I*R*δϕ
        r = [-R*cos(ϕ), -R*sin(ϕ), z]
        dB = cross(Idl, r)/norm(r)^3
        B = B + dB
    end
    B = round.(B.*factor.*1.0e9, sigdigits=6)
    return B
end  
{% endhighlight %}

and here are four tests os this code (the analytical result provides and easy check) using steps of 1e-5 radian.
A Python version of this code (using NumPy) takes over 130 seconds to complete, and on a 2016 MBP, this takes 1.2 seconds in Julia.

{% highlight julia %}
@time begin
println("Expect B(z)=621.3185 :", b_field(1.0,1.0,0.0,0.00001) )
println("Expect B(z)=314.159 :", b_field(1.0,2.0,0.0,0.00001) )
println("Expect B(z)~ 0 :", b_field(1.0,1.0,1.0e6,0.00001) )
println("Expect B(z)= 0 :", b_field(0.0,1.0,1.0e6,0.00001) )
end
{% endhighlight %}

