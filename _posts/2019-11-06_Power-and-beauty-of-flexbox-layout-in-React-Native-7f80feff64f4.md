# Power and beauty of flexbox layout in React Native

Flex is probably the single and most useful property that you will use all accross your React-Native app.

Flex is probably the single and most useful property that you will use all accross your React-Native app.

![](https://cdn-images-1.medium.com/max/1200/1*8brjhbwXZl30mNwXbcSw2g.jpeg)

Flex is probably the single and most useful property that you will use all accross your React-Native app.

Flex specifies the ability of a component to alter its dimensions to fill the space of the container it’s in. This value is relative to the flex properties specified for the rest of the items ( rest of the sibling items)within the same container.

In the web we have to specifically state display:flex, but all container elements in React Native are Flex containers by default.

We mostly assign `flex` to a container element or to the element itself to specify how much space the element should take from the available space, e.g. if we put `flex:1` this makes the element take all the available space in the axis, as the example below explains:

undefined
undefined![](https://cdn-images-1.medium.com/max/800/1*wmGL-6EXfKXIisbMYW0dJA.png)
undefined

So if we have a View element with a height of 300 and a width of 300, and a child View with a property of flex: 1, then the child view completely fills the parent view. If we decide to add another child element with a flex property of flex: 1, they’ll each take up equal space within the parent container. See below

undefined
undefined![](https://cdn-images-1.medium.com/max/800/1*c9F0morRVxVMixK8GDxE5g.png)
undefined

Another way to understand it is to think of the `flex` properties as being percentages. For example, if I want my two child components to take up 66.6% and 33.3% respectively, I could use `flex:66` and `flex:33`. The first child item occupies two-thirds, and the second child item occupies one-third of the parent container. The `flex` number is only important relative to the other `flex` items occupying the same space, i.e. other sibling items. Thinking of flex numbers as percentages makes it much easier to comprehend. Rather than `flex:66` and `flex:33`, I could have specified `flex:2` and `flex:1` and achieved the same layout effect.

Now conside the below example when the same parent element with flex of 1 will will have some child elements. And **adding flexDirection: ‘row’** to the parent container causes the children to be laid out horizontally. Note, if **flexDirection is** not sepcified then by default it takes ‘column’

undefined
undefined![](https://cdn-images-1.medium.com/max/800/1*uoXw705STKuEH0vh77BsrA.png)
undefined

#### Now lets first quickly understand the concept of cross axis or secondary in React or React Native

[**From Mozilla**](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox) **documentation says the below**

The cross axis runs perpendicular to the main axis, therefore if your `flex-direction` (main axis) is set to `row` or `row-reverse` the cross axis runs down the columns.

![](https://cdn-images-1.medium.com/max/800/0*LJawDhqT6jgosN3z.png)
undefined

If your main axis is `column` or `column-reverse` then the cross axis runs along the rows.

![](https://cdn-images-1.medium.com/max/800/0*swE7J55Z-mWu-Pnp.png)
undefined

This means that **_Flex container_ children always follows parent container Main Axis direction** and by changing flex-direction property we are changing Main Axis direction which in turn meas we are changing child elements alignment direction.

**Lets take a look at the most important alignment properties that we’ll use to control the Flexbox layout:**

**flex  
flexDirection  
justifyContent  
alignItems  
alignSelf  
flexWrap**

#### `alignItems`

`alignItems` aligns children in the **cross direction or cross-axis**. For example, if children are flowing vertically, `alignItems` controls how they align horizontally.

#### `justifyContent`

`justifyContent` defines how space is distributed between and around flex items along the primary axis of the container (the flex-direction). `justifyContent` is declared on the parent container.

#### So in short things to remember between `**alignItems and** justifyContent`

*   `flexDirection` determines the primary axis as ‘**row**’ or ‘**column**’.
*   `justifyContent` determines distribution of children along **primary axis**.
*   `alignItems` determines the **alignment** of children along the **secondary axis.**

So whats the difference between justifyContent and alignItems

`**justifyContent:**` **Horizontal or primary or main axis (X-axis)**

**Alignment & Spacing along primary axis (X-axis)**

`flex-start;` Align children _horizontally_ left

`flex-end;` Align children _horizontally_ right

`center;` Align children _horizontally_ centered _(amaze!)_

`space-between;` Distribute children _horizontally_ evenly across entire width

`space-around;` Distribute children _horizontally_ evenly across entire width (but with space on the edges

`**align-items:**` **Vertical or Cross axis**

**Alignment only along Cross or** **secondary axis (Y-axis)**

`flex-start;` Align children _vertically_ top

`flex-end;` Align children _vertically_ bottom

`center;` Align children _vertically_ centered _(amaze!)_

`baseline;` Aligned children _vertically_ so their baselines align _(doesn't really work)_

`stretch;` Force children to be height of container _(great for columns)_

**_The effect of justify-content and align-items changes depending on the flex-direction. E.g. if flex-direction is set to column, meaning primary axis is column, hence justify-content affects vertical alignment, and alignItems affects the cross-axis, horizontal alignment and vice versa._**

**_And if flex-direction is set to row, meaning primary axis is row, hence justify-content affects alignment in the direction of row i.e. horizontally.And alignItems affects the cross-axis, i.e. veritical alignment._**

Lets see this in action. Specifying `center` causes the children to be centered within the parent container. The free space is distributed on both sides of the clustered group of children.

undefined
undefined![](https://cdn-images-1.medium.com/max/800/1*mHpcHg0lwWz8F9j6uuLUew.png)
undefinedundefined
undefined![](https://cdn-images-1.medium.com/max/800/1*E9FDLUsMXnAryE7to8d7Xw.png)
undefinedundefined
undefined![](https://cdn-images-1.medium.com/max/800/1*AjGmQmeYyY9BsIUztShpsw.png)
undefined

**justifyContent takes five options :**

*   center
*   flex-start
*   flex-end
*   space-around
*   space-between

`flex-start` groups the components at the beginning of the flex `column` or `row` depending upon what value is assigned to `flexDirection`. `flex-start` is the default value for `justifyContent`.

`flex-end` acts in the opposite manner. It groups items together at the end of the container.

`space-around` attempts to evenly distribute space around each element. Meaning, Flexbox allocates the same amount of space on each side of the element

undefined
undefined![](https://cdn-images-1.medium.com/max/800/1*ZTO_z6VwL0sPtOFZ2W8lBA.png)
undefined

`space-between` doesn’t apply spacing at the start or end of the container. That is the first and the last items are kept at the very beginning and end of the whle space. But the space between the elements are The space between any two consecutive elements is the same as the space between any other two consecutive elements.

undefined
undefined![](https://cdn-images-1.medium.com/max/800/1*br0Q8UQHlbwa11PtLsq-Jg.png)
undefined

Now for the same example above, lets think of the situation when the width of individual elements are such that they dont fit in and items flow off the screen.

To control this we have the flexWrap property, `flexWrap` takes two values: `nowrap` and `wrap`. **The default value is** `**nowrap**`, meaning that items flow off the screen if they don’t fit, and the user can’t see them. And the other one is wrap, which will wrap the elements and make them visible inside the screen.

undefined
undefined![](https://cdn-images-1.medium.com/max/800/1*zchqSU7_sOftvRtIc15ggg.png)
undefined

And if I just put **_flexWrap: “wrap”_** in the parent container View component, then the result will be as below.

![](https://cdn-images-1.medium.com/max/800/1*tThKHqr7NqiCkz7wdLqcfQ.png)
undefined

### alignContent

If you went with `flexWrap:'wrap'` you have multiple lines of items, this property will help you align the lines on the cross-axis.

undefined
undefined![](https://cdn-images-1.medium.com/max/800/1*nDG2bpcFcN3-oQZvXjCIyQ.png)
undefined

#### Overriding the parent container’s alignment with alignSelf

Till now we have seen, all of the flex properties have been applied to the parent container, because **_Flex container_ children always follows parent container. But not with alignSelf,** it aligns an item along the **cross axis** **overwriting** ites **parent** alignItem property.

It takes the following options

`_‘flex-start’, ‘flex-end’, ‘center’, ‘stretch’, and ‘auto'_`

The default value for `alignSelf` is `auto`. `auto` takes the value from the parent container’s `alignItems` setting.

Lets see with an example

undefined
undefined![](https://cdn-images-1.medium.com/max/800/1*Xs5EjEJTCADdVF7bK63TyQ.png)
undefined

Now if I just uncomment the line **alignSelf: “flex-end”**

![](https://cdn-images-1.medium.com/max/800/1*GatwVuGbgii2f7hbcnlSUw.png)
undefined

By [Rohan Paul](https://medium.com/@paulrohan) on [November 6, 2019](https://medium.com/p/7f80feff64f4).

[Canonical link](https://medium.com/@paulrohan/power-and-beauty-of-flexbox-layout-in-react-native-7f80feff64f4)

Exported from [Medium](https://medium.com) on December 12, 2020.