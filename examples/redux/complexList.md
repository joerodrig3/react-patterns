# Complex List

In many cases, you may end up with a list that needs to keep in sync with the
Redux store. This pattern allows our list to keep in sync with our store, without
being dependent on the parent component that calls it(ie. A page container). This
is done on purpose. This solution reduces the amount of times props get passed
through components, allowing us to isolate actions and data to specific components. 

# Build the container

```javascript
import React, { Component, PropTypes } from "react";
import ComplexList from "./ComplexList";

class ComplexListContainer extends Component {
  static propTypes = {
    data: PropTypes.shape({items: Proptypes.array.isRequired}).isRequired,
    demoAction: PropTypes.func.isRequired,
  };

  render() {
    const { data, demoAction } = this.props;
    return <ComplexList items={data.items} demoAction={demoAction} />;
  }
}

function mapStateToProps({data}) {
  return {data};
}

function mapDispatchToProps(dispatch) {
  return {
    demoAction: () => dispatch(actions.demoAction()),
  };
}

export default connect(mapStateToProps, mapDispatchToProps)(ComplexListContainer);
```

# Build the list component

```javascript
import React, { Component, PropTypes } from "react";
import ComplexListItem from "./ComplexListItem";

// ComplexList 
export default class ComplexList extends Component {
  static propTypes = {
    items: Proptypes.arrayOf(Proptypes.shape({
        message: Proptypes.string.isRequired,
    })).isRequired,
    demoAction: PropTypes.func.isRequired,
    
    // Bindings
    this._complexListItems = this._complexListItems.bind(this);
    
  };

  _complexListItems() {
    const { items } = this.props.data;
    return items.forEach( (item) => <ComplexListItem item={item} /> );
  }

  render() {
    return this._complexListItems();
  }
}
```

# Build the list item Component

```javascript
import { PropTypes } from "react";

// ComplexListItem presentational component
export default function ComplexListItem(props) {
  const { message } = props;
  return <li>{message}</li>;
}

ComplexListItem.propTypes = {
  message: Proptypes.string.isRequired,
};
```
