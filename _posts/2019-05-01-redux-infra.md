---
layout: post
title: "How I do React / Redux"
tags: ["development", "react", "redux"]
---

Wherein I'll discuss how I setup React / Redux apps with loading and error status indicators.

Last night, I saw a demo of a new startup in stealth-mode.  I was impressed with their tech, but noticed that there weren't any loading indicators or error statuses shown.  I'm gonna show you how I set things up so that you can simply display when things are going, or going wrong.  

For this to work, you'll need to setup [redux-thunk](https://github.com/reduxjs/redux-thunk) and [lodash](https://lodash.com/) in your application.

As you will see, there are a bunch of opportunities to DRY up this code, especially as you lay down a bunch of code following this pattern.  For the sake of clarity, I'll be skipping brevity.

## Action Creators and Actions

In the action creator, we'll be dispatching two of three actions, one on start, and either a success or error action:

```javascript

export const GET_WIDGETS = "GET_WIDGETS";
export const GET_WIDGETS_SUCCESS = "GET_WIDGETS_SUCCESS";
export const GET_WIDGETS_ERROR = "GET_WIDGETS_ERROR";

export function getWidgets() {
    return (dispatch) => {
        dispatch({ type: GET_WIDGETS });

        return fetch('/api/widgets', { credentials: 'same-origin' })
            .then((response) => {
                if (!response.ok) {
                    let error = response.text();
                    dispatch({ type: GET_WIDGETS_ERROR, error: error });
                    return Promise.reject(error);
                }

                let data = response.json();
                dispatch({ type: GET_WIDGETS_SUCCESS, data: data });
                return Promise.resolve(data);
            })
            .catch((error) => {
                dispatch({ type: GET_WIDGETS_ERROR, error: error });
                return Promise.reject(error);
            })
    }
}

```

## Reducers

For the reducer associated with the above, we'll be storing any errors passed along with the action, along with a loading flag:

```javascript

const initialState = {
    loading: true,
    data: [],
    error: null
}

export function widgetList(state = initialState, action) {
    switch (action.type) {

        // loading action
        case GET_WIDGETS:
            return Object.assign({}, state, initialState);

        // success action
        case GET_WIDGETS_SUCCESS:
            return Object.assign({}, state, {
                loading: false,
                data: action.data,
                error: null
            });

        // error action
        case GET_WIDGETS_ERROR:
            return Object.assign({}, state, {
                loading: false,
                data: {},
                error: action.error
            });

        default:
            return state
    }
}

```

## React Components

Now, for the payoff.  We've got enough metadata with our store to allow us to create a nice way to encapsulate the states of stores needed:

```javascript

// Component that monitors multiple stores for status changes,
// and does the right things
export function StatusComponent(props) {
    let stores = _.isArray(props.stores) ? props.stores :
        [props.stores];

    // if any of the stores have an error,
    // show the error message
    let errors = _.map(stores, (store) => !_.isNil(store.error));
    if (_.compact(errors).length > 0) {
        return (<ErrorMessage />);
    }

    // if any of the stores are still loading,
    // show the loading spinner
    let loading = _.map(stores, (store) => store.loading);
    if (_compact(loading).length > 0) {
        return (<LoadingSpinner />);
    }

    return props.children;
}

function ErrorMessage(props) {
    ...
}

function LoadingSpinner(props) {
    ...
}

```

Which can be used like:

```javascript

    render() {
        return (
            <StatusComponent stores={ this.props.widgetList }>
                <div />
            </StatusComponent>
        )
    }

```

The only caveat is that nested components inside the `StatusComponent` will try to resolve parts of the stores, so `_.get` is your friend.  If anyone knows how to stop children from rendering, I'd love to know.  
