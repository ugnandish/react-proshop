# react-proshop
reactjs eCommerce application  

## react setup
npx create-react-app frontend<br/>
cd frontend<br/>
start your react application - **npm start** 

delete unwanted (not used) file from project - logo.svg, App.css, App.test.js, index.css 

**App.js**

```
const App = () => {
  return (
    <h1>App</h1>
  )
};
export default App;
```

and move the **.gitignore** file to root folder <br/>
and add below points <br/>
**node_modules <br/>
.env**

install react-bootstrap and icons <br/>
**npm i react-bootstrap bootstrap react-icons**

bootstrap file add in index.js file <br/>
**import 'bootstrap/dist/css/bootstrap.min.css'**

create **components** folder under src <br/>

create **Header.js** file under components <br/>
```
import react from 'react'
import {Navbar, Nav, Container} from 'react-bootstrap';
import {FaShoppingCart, FaUser} from 'react-icons/fa';

const Header = () => {
  return (
    <header>
      <Navbar bg='dark' variant='dark' expand='md' collapseOnSelect>
        <Container>
          <Navbar.Brand href='/'>ProShop</Navbar.Brand>
          <Navbar.Toggle aria-controls='basic-navbar-nav' />
          <Navbar.Collapse id='basic-navbar-nav'>
            <Nav className='ms-auto'>
              <Nav.Link href='/cart'>
                <FaShoppingCart /> Cart
              </Nav.Link>
              <Nav.Link href='/login'>
                <FaUser />Sign In
              </Nav.Link>
            </Nav>
          </Navbar.Collapse>
        </Container>
      </Navbar>
    </header>
  )
}
export default Header;
```

create **Footer.js** file under components
```
import React from 'react';
import  {Container, Row, Col} from 'react-bootstrap';
const Footer = () => {
  const currentYear = new Date().getFullYear();
  return (
    <footer>
      <Container>
        <Row>
          <Col className='text-center py-3'>
            <p>ProShop &copy; {currentYear}</p>
          </Col>
        </Row>
      </Container>
    </footer>
  )
}
export default Footer;
```

Add **Header & Footer** in App.js
```
import React from 'react'
import Header from './components/Header';
import { Container } from 'react-bootstrap';
import Footer from './components/Footer';

const App = () => {
  return (
    <>
      <Header />
      <main className='py-3'>
        <Container>
          <h1>welcome to proshop</h1>
        </Container>
      </main>
      <Footer/>
    </>
  )
}
export default App;
```

## List Products
create **images** folder under public <br/>
and create **products.js** file under src <br/>
Example: **products.js** <br/>

```
const products = [
  {
    _id: '1',
    name: 'Airpods Wireless Bluetooth Headphones',
    image: '/images/airpods.jpg',
    description:
      'Bluetooth technology lets you connect it with compatible devices wirelessly High-quality AAC audio offers immersive listening experience Built-in microphone allows you to take calls while working',
    brand: 'Apple',
    category: 'Electronics',
    price: 89.99,
    countInStock: 10,
    rating: 4.5,
    numReviews: 12,
  },
  etc....
]
export default products;
```

create **screens** folder under src <br/>
create **HomeScreen.js** file under screens
```
import React from 'react';
import products from '../products';
import {Row, Col} from 'react-bootstrap';

const HomeScreen = () => {
  return (
    <>
      <h1>Latest Products</h1>
      <Row>
        {products.map((product) => (
          <Col sm={12} md={6} lg={4} xl={3}>
            <h3>{product.name}</h3>
          </Col>
        )) }
      </Row>
    </>
  )
}

export default HomeScreen
```

call HomeScreen.js in App.js
```
import HomeScreen from './screens/HomeScreen';
....
<Container>
  <HomeScreen />
</Container>
....
```

create **Product.js** file under components <br/>
react-router-dom not yet added so using a tag
```
import React from 'react';
import {Card} from 'react-bootstrap';

const Product = ({product}) => {
  return (
    <Card className='my-3 p-3 rounded'>
      <a href={`/product/${product._id}`}>
        <Card.Img src={product.image} variant='top' />
      </a>

      <Card.Body>
        <a href={`/product/${product._id}`}>
          <Card.Title as='div'>
            <strong>{product.name}</strong>
          </Card.Title>
        </a>

        <Card.Text as='h3'>
          ${product.price}
        </Card.Text>
      </Card.Body>
    </Card>
  )
}
export default Product
```

and update the HomeScreen.js file
```
import Product from '../components/Product';
<>
  <h1>Latest Products</h1>
  <Row>
    {products.map((product) => (
      <Col key={product._id} sm={12} md={6} lg={4} xl={3}>
        <Product product={product} />
      </Col>
    )) }
  </Row>
</>
```

## Implement React Router
install **react-router-dom** <br/>
**npm i react-router-dom**
install **react-router-bootstrap** <br/>
**npm i react-router-bootstrap**

**index.js** <br />
```
import React from 'react';
import ReactDOM from 'react-dom/client';
import 'bootstrap/dist/css/bootstrap.min.css';
import { createBrowserRouter, createRoutesFromElements, Route, RouterProvider} from 'react-router-dom';
import App from './App';
import HomeScreen from './screens/HomeScreen';
import reportWebVitals from './reportWebVitals';

const router = createBrowserRouter (
  createRoutesFromElements (
    <Route path='/' element={<App/>}>
      <Route index={true} path='/' element={<HomeScreen/>} />
    </Route>
  )
)

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);

reportWebVitals();
```

HomeScreen added in index.js file so removed from App.js <br/>
**App.js**
```
import {Outlet} from 'react-router-dom'
....
<Container>
  <Outlet />
</Container>
....
```
 
and **replace "a" to "Link"** with react-router-dom <br/>
**product.js**
```
import { Link } from 'react-router-dom';
....
<Link to={`/product/${product._id}`}>
  <Card.Img src={product.image} variant='top' />
</Link>
....
```

add **LinkContainer instead of href**  <br/>
**Header.js** <br/>
```
import {LinkContainer} from 'react-router-bootstrap';
....
<LinkContainer to='/'>
  <Navbar.Brand>ProShop</Navbar.Brand>
</LinkContainer>
....
```

## Rating component 
create **Rating.js** under component 
```
import React from 'react'
import { FaRegStar, FaStar, FaStarHalfAlt } from 'react-icons/fa'

const Rating = ({value, text}) => {
  return (
    <div className='rating'>
      <span>
        { value>=1 ? <FaStar /> : value >= 0.5 ? <FaStarHalfAlt /> : <FaRegStar /> }
      </span>
      <span>
        { value>=2 ? <FaStar /> : value >= 1.5 ? <FaStarHalfAlt /> : <FaRegStar /> }
      </span>
      <span>
        { value>=3 ? <FaStar /> : value >= 2.5 ? <FaStarHalfAlt /> : <FaRegStar /> }
      </span>
      <span>
        { value>=4 ? <FaStar /> : value >= 3.5 ? <FaStarHalfAlt /> : <FaRegStar /> }
      </span>
      <span>
        { value>=5 ? <FaStar /> : value >= 4.5 ? <FaStarHalfAlt /> : <FaRegStar /> }
      </span>
      <span className='rating-text'> { text && text } </span>
    </div>
  )
} 

export default Rating
```

**add Rating.js in Product.js**
```
import Rating from './Rating';
....
<Card.Text as='div'>
  <Rating value={product.Rating} text={`${product.numReviews} reviews`} />
</Card.Text>
```

add respective styling in src/assets/styles/index.css
