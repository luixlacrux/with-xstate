# (HOC) With XState

## Installation

```bash
npm install with-xstate
or
yarn add with-xstate
```

## Require

- [xstate](https://github.com/davidkpiano/xstate)

## Usage

In order to use `withXState` first pass the machine state through a prop to your component and then wrap it

### mapActionsToXState((dispatch, ownProps) => {...})

takes a function that should return and object that contains a key, value pair, where each key is a machine state and its value should be another object that contain a key, value pair with the actions and the functions that should be execute once xstate added to the next state.

### mapActionsToXState receive two optionally parameters (dispatch, onwProps)

- As first parameter is `dispatch` that you can use to dispatch redux actions in your state actions.
- If the `ownProps` parameter is specified, with-xstate will pass the props that were passed to the component.

### Usage with react

```js
import { Machine } from 'xstate';
import withXState from 'with-xstate';
import Component from './Component';

const toggleMachine = Machine({
  initial: 'inactive',
  states: {
    inactive: {
      on: {
        TOGGLE: 'active',
        onEntry: ['fetchQuery']
      }
    },
    active: { on: { TOGGLE: 'inactive' } }
  }
});

const mapActionsToXState = (dispatch, ownProps) => ({
  machineState: {
    fetchQuery: () => {
      console.log('fetch...');
    }
  }
});

const WithXStateComponent = withXState(mapActionsToXState)(Component);

<WithXStateComponent machineState={toggleMachine} />;
```

### Usage with react-redux

```js
import { connect } from 'react-redux';
import withXState from 'with-xstate';
import Component from './Component';

const mapActionsToXState = () => ({
  machineState: {
    fetchQuery: () => {
      console.log('fetch...');
    }
  }
});

const WithXStateComponent = withXState(mapActionsToXState)(Component);

const mapStateToProps = state => ({
  machineState: state.machineState // your state machine from redux
});

const WithConnectComponent = connect(mapStateToProps)(WithXStateComponent);
```
