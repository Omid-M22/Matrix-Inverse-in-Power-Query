# Matrix-Inverse-in-Power-Query
with this custom function, you can easily calculate the inverse of matrixes, no mater the dimension of matrixes


Previousely I have provided several custom functions for the Matrix and Vector calculation on Power Query, available at [Matrix & Vector Calculation](https://github.com/Omid-M22/Matrix-and-Vector-Calculations-in-Power-Query-)

But here I have provided the most chalenging one for the calculation of Mateixes inverse as below

```powerquery-m

(Matrix as table) as table=>
let
       Dim=Table.RowCount(Matrix),
      FX= (V1,V2,a)=>List.Transform(List.Positions(V1), each Number.IntegerDivide(V1{_} +a*V2{_},0.000001)/1000000),
    MatrixEye=Table.FromColumns( List.Split(List.Transform({1..Dim*Dim}, each if Number.Mod(_,Dim+1)=1 then 1 else 0),Dim)),
    Data=Table.ToColumns(Table.Transpose(Matrix)&MatrixEye),
    Custom3 = List.Accumulate({0..Dim-1},Data,(x,y)=>List.Transform({0..Dim-1}, each if _ =y then List.Transform(x{y},(z)=>z/x{y}{y}) else FX(x{_},x{y},-x{_}{y}/x{y}{y}))),
    Result = Table.Transpose(Table.Skip(Table.FromColumns(Custom3),Dim))
in
    Result


```
