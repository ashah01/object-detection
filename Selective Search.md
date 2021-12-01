[Selective Search for Object Detection (C++ / Python)](https://learnopencv.com/selective-search-for-object-detection-cpp-python/)
[Selective Search for Object Recognition](http://www.huppelen.nl/publications/selectiveSearchDraft.pdf)
[Efficient Graph-Based Image Segmentation](http://cs.brown.edu/people/pfelzens/papers/seg-ijcv.pdf)

### Object recognition considerations
- **Capture all scales**: images will contain objects of varying size and shapes, encapsulating all of them is critical to accurate representations.
- **Diversification**: There's no absolute optimal strategy to clump region proposals together. Factors like colour, brightness, texture, etc. affect how local regions form an object within an image.
- **Speed of computation**: Optimally we'd be able to rely on a method as consistent as exhaustive search and all its power, but in reality we have a limited amount of computational resources and must stay within a reasonable limit.

### Selective Search by Hierarchical Grouping
A bottom-up hierarchical grouping (clustering) algorithm forms the basis of this implementation. Due to the fact that grouping itself is a hierarchical task, we can generate locations at all scales by simply extending the recursive nature of the algorithm until it's reached the full image scale.

![Untitled](img/Untitled_4.png)

To obtain an initial set of small feature regions, we'll use Felzenszwalb and Huttenlocher's [[Efficient Graph-Based Image Segmentation]] algorithm. Our approach can now be detailed as
```
Input: (colour) image  
Output: Set of object location hypotheses L

Obtain initial regions R = {r1,··· ,rn} using EGBIS
Initialise similarity set S = 0
foreach Neighbouring region pair (ri,rj) do
	Calculate similarity s(ri,rj) 
	S = S∪s(ri,rj)

while S ≠ 0 do  
	Get highest similarity s(ri, rj) = max(S)  
	Merge corresponding regions rt = ri∪rj  
	Remove similarities regarding ri : S = S\s(ri, r∗)
	Remove similarities regarding rj : S = S\s(r∗,rj)
	Calculate similarity set St between rt and its neighbours
	S=S∪St  
	R=R∪rt

Extract object location boxes L from all regions in R

```

### Diversification Strategies
