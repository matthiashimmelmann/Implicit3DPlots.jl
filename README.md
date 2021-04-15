# Implicit 3D Plots

Visualizing parametrized space curves or surfaces is not complicated: We simply need to iterate the parametrization's domain (usually <img src="https://render.githubusercontent.com/render/math?math=\mathbb{R}">) and get a reasonably smooth plot as a result. Implicitly defined surfaces and curves pose a problem however: If we were to proceed in a similar manner, we would have to iterate through <img src="https://render.githubusercontent.com/render/math?math=\mathbb{R}^3"> and check, whether the implicit equations are 0 in any of the points. This approach is problematic in two ways. Firstly, numerics and potentially instable implicit functions prohibit this method: It is unlikely, that we will find rational points that actually lie on the curve or surface in this way. Generally, this method will only yield points that *approximately* lie on the implicit set. However, this we could cope with: the grid can be chosen arbitrarily small, so thoretically we could reach machine presision with this method - at the expense of a reasonable runtime. More problematically however, there is no natural notion of what point *comes next* on the curve or surface. In the case of parametrized curves, we always know the next point on the curve, because there is a natural total order on <img src="https://render.githubusercontent.com/render/math?math=\mathbb{R}">. Finding the next point on a parametrized curve is harder, as the proximity of points is an insufficient criterion in this setting, as the closest point might not be unique. 

Drawing inspiration from the Julia package [ImplicitPlots.jl](https://github.com/saschatimme/ImplicitPlots.jl "ImplicitPlots.jl") there is a way out from this dilemma though. Using the library [Meshing.jl](https://github.com/JuliaGeometry/Meshing.jl "Meshing.jl") immediately enables us to sample an implicit surface and form a mesh between the points that covers the surface. For implicit space curves, let us recall that if the space curve can be represented by two equations, we can simply intersect the two corresponding surfaces to obtain the space curve. For this reason, we can utilize the previously mentioned meshes and intersect them. 

## Usage

There are two main methods in this package: `plot_implicit_surface` and `plot_implicit_curve`. Let us first consider an example of the former:

```
julia> f(x) = x[1]^2+x[2]^2-x[3]^2-1
julia> scene = plot_implicit_surface(f; transparency=false)
```

The result of this can be seen in the following image: 
<p align="center">
  <img src="https://user-images.githubusercontent.com/65544132/114864346-2b0ec700-9df1-11eb-8ad4-4ef2d4e1c9f3.png" width="600", height="600">
</p>

Notice that the standard options are that the plot's color is `:steelblue`, the plot is transparent, the surface's shading is activated and the standard meshing method is `MarchingCubes`, because it is slightly faster than `MarchingTetrahedra`.

As an example of the latter method, let us consider the input

```
julia> f(x)=x[1]^2+x[2]^2-x[3]^2-1
julia> g(x)=x[1]^2+x[2]^2+x[3]^2-2
julia> plot_implicit_curve(f, g)
```

which produces the following picture:
<p align="center">
  <img src="https://user-images.githubusercontent.com/65544132/114867917-a1152d00-9df5-11eb-9b38-b1fd69a8ce8f.png" width="600", height="600">
</p>

In this context, it is important to notice that the two implicit surfaces generated by the implicit equations need to lie in relative general position. If they are only kissing, the mesh might not be able to reflect their intersection properly.
