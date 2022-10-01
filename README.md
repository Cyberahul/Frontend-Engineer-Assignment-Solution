# Frontend-Engineer-Assignment-Solution
### Q-1: Explain what the simple List component does.
Ans:

Lists are handy when it comes to developing the UI of any website. Lists are mainly used for displaying menus on a website, for example, the navbar menu. In conventional JavaScript, we can use arrays for creating lists. We can produce lists in React in a similar manner as we do in standard JavaScript. 
#### Example.

    const SimpleList = () => (
    {['a', 'b', 'c'].map(function(item) { return
    {item}
    ; })} 
    
Basically, this List Component is used to display an unordered List with some texts as List items. WrappedListComponent creates an unordered list. It takes an array (items) of strings(text) as props. For each item in the array, it maps through all and displays their content on the screen with a background color of 'Red'. If one item is selected, then it displays it with a different background color (Green). The user can click on an item and it will get selected and will get background color of Green.


### Q-2: What problems / warnings are there with code?
Ans:

  1-> **array** should be **arrayOf** as we are defining type of array elements as well. **shapeOf** should be **shape(syntax error)**.
  
  ![image](https://user-images.githubusercontent.com/63242259/193395325-12975507-a3ed-4db3-bf74-90f84a1af160.png)

  2-> following the convention, **useState** variables being misplaced. the return value of **useState** should be destructured like this: **[selectedIndex, setSelectedIndex] =             useState()**
  
  ![image](https://user-images.githubusercontent.com/63242259/193395359-59bde51b-d99d-48e4-af86-3720b2fe2d6a.png)

  3-> the **isSelected** prop expects a boolean value. I was able to catch this semantic error through propTypes defined for the **SingleListItem** component. Here is one possible       solution: **isSelected={selectedIndex === index}**
  
  ![image](https://user-images.githubusercontent.com/63242259/193395380-855b5ea1-3f18-49dc-b24e-5b052517cdaa.png)

  4-> **onClick** events should have a function reference instead of a function call. 
  
    <li style={{ backgroundColor: isSelected ? "green" : "red" }}
     onClick={onClickHandler(index)}>
      {text}</li>
  
  5-> Passing a number **selectedIndex** to **isSelected** which should be a bool.
  
    <ul style={{ textAlign: "left" }}>
       {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>

  6-> Defining a default prop as null is not recommended.
    
    WrappedListComponent.defaultProps = {
    items: null,
    };

  7-> Unique key prop is missing for List items.
  
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>

### Q-3: Please fix, optimize, and/or modify the component as much as you think is necessary.
Ans:
```javascript
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
    index,
    isSelected,
    onClickHandler,
    text,
}) => {
    return (
        <li
            style={{ backgroundColor: isSelected ? 'green' : 'red' }}
            // onClick={onClickHandler(index)} //There should be a function reference instead of call
            onClick={() => onClickHandler(index)}
        >
            {text}
        </li>
    );
};

WrappedSingleListItem.propTypes = {
    index: PropTypes.number,
    isSelected: PropTypes.bool,
    onClickHandler: PropTypes.func.isRequired,
    text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items, }) => {

    // const [setSelectedIndex, selectedIndex] = useState();the place of selectedIndex & setSelectedIndex should get interchanged.
    const [selectedIndex, setSelectedIndex] = useState();

    useEffect(() => {
        setSelectedIndex(null);
    }, [items]);

    const handleClick = index => {
        setSelectedIndex(index);
    };

    return (
        <ul style={{ textAlign: 'left' }}>
            {items.map((item, index) => (
                <SingleListItem
                    key={index}
                    onClickHandler={() => handleClick(index)}
                    text={item.text}
                    index={index}
                    // isSelected={selectedIndex}
                    isSelected={Boolean(selectedIndex)}
                />
            ))}
        </ul>
    )
};

// WrappedListComponent.propTypes = {
//     items: PropTypes.array(PropTypes.shapeOf({
//         text: PropTypes.string.isRequired,
//     })),
// };

//The above code contains some errors, The modified code is shown below

WrappedListComponent.propTypes = {
    items: PropTypes.arrayOf(PropTypes.shape({
        text: PropTypes.string.isRequired,
    }).isRequired
    ).isRequired
};

WrappedListComponent.defaultProps = {
    // items: null //giving null as default prop is never recommended
    items: undefined 
};

const List = memo(WrappedListComponent);

export default List; ```
