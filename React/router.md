# Router

Install React Router with npm: `npm install --save react-router-dom@5.2.0`

> Note: The most recent version is v6 so there may be notes reflecting differences.

Import the [BrowserRouter](https://v5.reactrouter.com/web/api/BrowserRouter) into the project:

```react
import { BrowserRouter as Router } from 'react-router-dom'
```

You then need to have `Router` be the top-level component in the return of the application. Then add `Route` to the same import, and include `Route` along with the `path`. If the path is not specified, then the component will always render.

```react
<Router>
			<Header /> <-- will always render
			<main>
				<Route path="/about">
					<About /> <-- will only render when the path /about follows the domain
				</Route>
				<Route path="/sign-up">
					<SignUp />
				</Route>
				<Route path="/articles">
					<Articles />
				</Route>
				<Route path="/categories">
					<Categories />
				</Route>
				<Route path="/profile">
					<Profile />
				</Route>
			</main>
			<Footer /> <-- will always render
		</Router>
```

## Linking to Routes

Rather than using the anchor `<a>` to link to other pages (which causes the page to reload), Router offers `Link` and `NavLink`, which are also imported from the same location. 

- They have a `to` prop that indicates the location to redirect the user to (similar to the anchor tag's `href` attribute)

- They wrap some HTML to use as the display for the link

  ```react
  <Link to="/about">About</Link>
  <NavLink to="/about">About</NavLink>
  ```

When the URL path matches a `NavLink` component's `to` prop, the link will automatically have an `'active'` class applied to it. It also accepts an optional `activeClassName` prop which can specify an additional class or set of classes that will be applied when the URL path matches their `to` prop.

## Dynamic Routes

Rather than specifying a unique route for every page, React Router allows us to express the pattern at a high level with a single route that can match any path by using URL parameters to create dynamic routes. URL parameters are dynamic segments of a URL that act as placeholders for more specific resources the URL is meant to display. They appear in a dynamic route as a colon `:` followed by a variable name.

```react
<Route path='/articles/:title'>
  <Article />
</Route>
```

A single route can even have multiple parameters (eg. `'/articles/:title/comments/:commentId'`) or none (eg. `'/articles'`).

To make a URL parameter optional, you can append a `'?'` to the end of the URL parameter's name tag (eg. `'/articles/:title?'`). In this case, the child component of the `Route` will render when the URL matches either `/articles/what-is-react` or just `/articles`.

## useParams

React Router provides a hook, `useParams()`, for accessing the value of URL parameters. When called, `useParams()` returns an object that maps the names of URL Parameters to their values in the current URL. This hook is also imported from the same import location of `react-router-dom`.

```react
import { Link, useParams } from 'react-router-dom';
 
// Rendered when the user visits '/articles/objects'
export default function Article() {
 
  let { title } = useParams();
  // title will be equal to the string 'objects'
 
  // The title will be rendered in the <h1>
  return (
    <article>
      <h1>{title}</h1>
      // ...
    </article>
  );
}
```

## Switch and exact

> As of v6, `Switch` changed to `Routes`

React Router provides several mechanisms for preventing several pages to render when the paths match more than once. The first is the `Switch` component that is imported from `react-router-dom`. When wrapped around a collection of routes, `Switch` will render the first of its child routes whose `path` prop matches the current URL. Make sure to list routes from most- to least- specific.

```react
<Switch>
  <div>
    <Route path='/articles/new'>
      <NewArticle />
    </Route>
    <Route path='/articles/:title'>
      <Article />
    </Route>
    <Route path='/articles'>
      <Articles />
    </Route>
  </div>
</Switch>
```

You can also use the `exact` prop to ensure that the route will match only if the current URL is an exact match.

```react
<Route exact path='/'>
   <Home />
 </Route>
```

## Nested Routes

Because React Router handles routing dynamically (eg. routes exist when they are rendered), you can render a `Route` anywhere within your application. In this case, we can relocate the `Route` that renders an individual `Category` component to within the `Categories` component where the links to that route are defined.

```react
import { Link, Route } from 'react-router-dom'
 
function Categories ({ categories }) {
  return (
    <div>
      <ul>
        {
          categories.map(category => 
            <li>
              <Link to={`/categories/${category}`}>
                {category}
              </Link>
            </li>
          )
        }
      </ul>
 
      <Route path={'/categories/:categoryName'}>
        <Category />
      </Route>
    </div>
  )
}

```

## useRouteMatch

Instead of writing out the full URL path, it would be much more flexible if we could create relative paths based on the `/categories` URL. React Router provides a hook, `useRouteMatch()`, that makes it easier to do.

```react
import { useRouteMatch, Link, Route } from 'react-router-dom';
import { SongPage } from '../SongPage.js'
 
function BandPage ({ songs }) {
  let { path, url } = useRouteMatch();
 
  // path = '/band/:band'
  // url = '/band/queen' 
 
  // Render a list of relative Links and a Route to render a SongPage
  return (
    <div>
      <ul>
        {
          songs.map(songName =>
            <li>
              <Link to={`${url}/song/${songName}`}> 
                {category}
              </Link>
            </li>
          )
        }
       </ul>
 
       <Route path={`${path}/song/:songName`}>
         <SongPage />
       </Route>
     </div>
  )
}
```

## Redirect

> As of v6, `Redirect` changed to `Navigate`

The `Redirect` component provided by React Router makes redirecting a user easy. Like a `Link` or `NavLink`, the `Redirect` component has a `to` prop. However, once the `Redirect` is rendered, the user will immediately be taken to the location specified by the `to` prop.

```react
import { Redirect } from 'react-router-dom'
 
const UserProfile = ({ loggedIn }) => {
  if (!loggedIn) {
    return (
      <Redirect to='/' />
    )
  }
 
  return (
    // ... user profile contents here
  )  
}
```



## useHistory

> As of v6, `useHistory` changed to `useNavigate`

React Router also provides a mechanism for updating the browser's location imperatively: the `Router`'s `history` object which is accessible via the `userHistory()`' hook.

The `history` object that `useHistory()` returns has a number of methods for imperatively redirecting users. The first and most straightforward is `history.push(location)` which redirects the user to the provided `location`.

```react
import { useHistory } from `react-router-dom`
 
export const ExampleForm = () => {
 
  const history = useHistory()
 
  const handleSubmit = e => {
    e.preventDefault();
    history.push('/')
  }
 
  return (
    <form onSubmit={handleSubmit}>
      {/* form elements */ }
    </form>
  )
}
```

Internally, the `BrowserRouter`'s `history` uses the [html5 history API](https://developer.mozilla.org/en-US/docs/Web/API/History_API). In brief, browser `history` is a stack that stores the URLs visited by the user and maintains a pointer to the user's current location. This `history` API allows you to navigate through a user's session history and alter the history stack if necessary.

In addition to `history.push()`, the `history` object has a few more useful methods for navigating through the browser's history:

- `history.goBack()` which navigates to the previous URL in the history stack
- `history.goForward()` which navigates to the next URL in the history stack
- `history.go(n)` which navigates `n` entries (where positive `n` values are forward and negative `n` values are backward) through the history stack

```react
import { useHistory } from `react-router-dom`
 
export const BackButton = () => {
  const history = useHistory()
 
  return (
    <button onClick={() => history.goBack()}>
      Go Back
    </button>
  )
}
```

## Query Parameters

Query parameters appear in URLs beginning with a question mark and are followed by a parameter name assigned to a value. They are optional and are more often used to earch, sort, and/or filter resources. React Router provides a mechanism for grabbing the values of query parameters: the `useLocation()` hook. When called, `useLocation()` returns a `location` object with a `search` property that is often directly extracted with destructuring syntax. This `search` value corresponds to the current URL's query string.

```react
import { useLocation } from 'react-router-dom'
 
// Rendered when a user visits "/list?order=DESC"
export const SortedList = (numberList) => {
  const { search } = useLocation();
  const queryParams = new URLSearchParams(search);
  const sortOrder = queryParams.get('order');
  console.log(sortOrder); // Prints 'DESC'
};
```

- First, we import `useLocation()` and call it within `SortedList` to get the `search` query parameter string `?order-DESC`
- Next, we pass the `search` string into the `new URLSearchParams()` constructor which returns an object, often called `queryParams`. This object has a `.get()` method for retrieving query paramter values
- Finally, to get the value of a specific query paramter, we pass in the name of the query paramter whose value we want to obtain as a string (`order`) to the `queryParams.get()`  method. The value (`DESC`) is then stored in the variable `order`.

Now expand the `SortedList` example so that the component uses the `order` value to render a list of data either in ascending order, in descending order, or in its natural order.

```react
if (sortOrder === 'ASC') {
    // render the numberList in ascending order
  } else if (sortOrder === 'DESC') {
    // render the numberList in descending order
  } else {
    // render the numberList as is
  }
```

