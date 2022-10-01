## Steel-Eye-Assignment-

#Q1. Explain what the simple List component does.

#ans:- The List variable stores a memoized instance of WrappedListComponent.

#Q2. What problems / warnings are there with code?

#ans:- #(A)  selectedIndex state in WrappedListComponent had confusing naming as when destructuring the array we were naming setState function as selectedIndex. As useState returns, an array in which the first element is the current value of the state and the second one is the setter function.

#(B)  When defining propTypes for WrappedListComponent were using the wrong syntax for defining array and object, as it should be `arrayOf` and `shape` respectively.

#(C) The default value of `items` for WrappedListComponent should be a empty array, so that when item is undefined nothing is rendered as items wont have anything inside it.

#Q3. Please fix, optimize, and/or modify the component as much as you think is necessary.

```
import React, { useState, useEffect, memo } from "react";
import PropTypes from "prop-types";

// Single List Item
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? "green" : "red" }}
      onClick={onClickHandler(index)}
    >
      {text}
    </li>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({ items }) => {
  const [selectedIndex, setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
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
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired
    })
  )
};

WrappedListComponent.defaultProps = {
  items: []
};

const List = memo(WrappedListComponent);

export default List;

```





