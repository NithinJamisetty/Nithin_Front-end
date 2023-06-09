1.A user interface element called the simple List component is used to show a group of items in a vertical list format. The list can be scrolled up or down to display more things, and each item is often denoted by a single line of text or a small icon.In order to obtain the desired visual appearance, the basic List component is frequently implemented in web development utilising HTML unordered lists (UL) or ordered lists (OL) and stylized with CSS. The List component is frequently used in mobile app development to provide menu options, navigational objects, or a list of options from which to choose.

2.1)The setSelectedIndex function in the WrappedListComponent component is being called instead of being assigned to the selectedIndex state variable, which is causing a runtime error. The line should be updated to const [selectedIndex, setSelectedIndex] = useState(null);.
The isSelected prop in the WrappedSingleListItem component is being passed the selectedIndex state variable, which is a number, instead of a boolean. It should be updated to isSelected={selectedIndex === index}.
The PropTypes for the items prop in the WrappedListComponent component is incorrect. It should be items: PropTypes.arrayOf(PropTypes.shape({ text: PropTypes.string.isRequired })) instead of items: PropTypes.array(PropTypes.shapeOf({ text: PropTypes.string.isRequired })).
The onClickHandler prop in the SingleListItem component is not being passed a function, but instead the return value of a function. It should be updated to onClick={() => onClickHandler(index)}.
The SingleListItem and WrappedListComponent components are being wrapped with the memo function unnecessarily, as they don't have any expensive computations that would benefit from memoization.


3.import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red' }}
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
const WrappedListComponent = ({
  items
}) => {
  const [selectedIndex, setSelectedIndex] = useState(null);

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = (index) => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        <SingleListItem
          key={index}
          onClickHandler={handleClick}
          text={item.text}
          index={index}
          isSelected={selectedIndex === index}
        />
      ))}
    </ul>
  );
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(
    PropTypes.shape({
      text: PropTypes.string.isRequired,
    })
  ).isRequired,
};

WrappedListComponent.defaultProps = {
  items: [
    {
      text: "Apple"
    },
    {
      text: "Mango"
    },
    {
      text: "Orane"
    }
  ]
};

const List = memo(WrappedListComponent);

export default List;









