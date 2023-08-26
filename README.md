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

## product details screen
create **ProductScreen.js** file under screens
```
import React from 'react'
import { useParams } from 'react-router-dom'
import { Link } from 'react-router-dom';
import {Row, Col, Image, ListGroup, Card, Button} from 'react-bootstrap';
import Rating from '../components/Rating';
import products from '../products'

const ProductScreen = () => {
  const {id:productId} = useParams();
  const product = products.find((p) => p._id === productId);
  console.log(product);

  return (
    <>
      <Link className='btn btn-light my-3' to='/'>
        Go Back
      </Link>
      <Row>
        <Col md={5}>
          <Image src={product.image} alt={product.name} fluid />
        </Col>
        <Col md={4}>
          <ListGroup variant='flush'>
            <ListGroup.Item>
              <h3>{product.name}</h3>
            </ListGroup.Item>
            <ListGroup.Item>
              <Rating value={product.rating} text={`${product.numReviews} reviews`} />
            </ListGroup.Item>
            <ListGroup.Item>Price: ${product.price}</ListGroup.Item>
            <ListGroup.Item>Description: ${product.description}</ListGroup.Item>
          </ListGroup>
        </Col>
        <Col md={3}>
          <Card>
            <ListGroup variant='flush'>
              <ListGroup.Item>
                <Row>
                  <Col>Price:</Col>
                  <Col>
                    <strong>${product.price}</strong>
                  </Col>
                </Row>
              </ListGroup.Item>
              <ListGroup.Item>
                <Row>
                  <Col>Status:</Col>
                  <Col>
                    <strong>${product.countInStock > 0 ? 'In Stock' : 'Out of Stock'}</strong>
                  </Col>
                </Row>
              </ListGroup.Item>
              <ListGroup.Item>
                <Button className='btn-block' type='button' disabled={product.countInStock === 0}>
                  Add to Cart
                </Button>
              </ListGroup.Item>
            </ListGroup>
          </Card>
        </Col>
      </Row>
    </>
  )
}

export default ProductScreen
```

and add the **ProductScreen.js in index.js** 
```
....
import ProductScreen from './screens/ProductScreen';
....
<Route path='/' element={<App/>}>
  <Route index={true} path='/' element={<HomeScreen />} />
  <Route  path='/product/:id' element={<ProductScreen />} />
</Route>
```

# Section 3: Serving and Fetching Data
create new folder **"backend"** under root folder<br/>
create new file **"server.js"** under backend folder

**install express** <br/>
**"npm i express"** under root folder

npm init - from root folder <br/>
new package.json file created <br/>
**package.json**
```
{
  "name": "proshop-v2",
  "version": "2.0.0",
  "description": "eCommerce application built with the MERN stack",
  "type": "module",
  "main": "server.js",
  "scripts": {
    "start": "node backend/server.js"
  },
  "author": "nandish",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

**server.js**
```
import express  from "express";
const port = 5000;
const app = express();
app.get('/', (req, res) => {
    res.send('API is running...')
});
app.listen(port, () => console.log(`server running on port ${port}`));
```

create **data** folder under backend <br/>
create **products.js** under data (copy the products.js data from frontend and paste it here)

**server.js**
```
import express  from "express";
import products from "./data/products.js";
const port = 5000;
const app = express();

app.get('/', (req, res) => {
    res.send('API is running...')
});
app.get('/api/products', (req, res) => {
    res.json(products);
});
app.get('/api/products/:id', (req, res) => {
    const product = products.find((p) => p._id === req.params.id);
    res.json(product);
});
app.listen(port, () => console.log(`server running on port ${port}`));
```

### Nodemon & Concurrently
install nodemon - **"npm i -D nodemon concurrently"** <br/>
in package.json update the client, server and dev
```
"scripts": {
    "start": "node backend/server.js",
    "server": "nodemon backend/server.js",
    "client": "npm start --prefix frontend",
    "dev": "concurrently \"npm run server\" \"npm run client\""
  }
```
### Environment Variables
**install dotenv** under root folder <br/>
**npm i -D dotenv** <br/>
create **.env** file under root folder <br/>
**.env** <br/>
```
NODE_ENV=development
PORT=5000
```

**server.js**
```
.....
import dotenv from "dotenv";
dotenv.config();
import products from "./data/products.js";
const port = process.env.PORT || 5000;
....
```

### Fetch Products
Fetching data using **axios** <br/>
install **axios under frontend** folder <br/>
frontend -> **npm i axios**

frontend > package.json <br/>
add "proxy": "http://localhost:5000" (only dev environment, not used in production) 
```
....
"private": true,
  "proxy": "http://localhost:5000",
  "dependencies": {
....
```

**HomeScreen.js** - \frontend\src\screens\HomeScreen.js <br/> 
fetch data from the server(backend) with the help of axios 
```
import React from 'react';
import {Row, Col} from 'react-bootstrap';
import { useEffect, useState } from 'react';
import Product from '../components/Product';
import axios from 'axios';

const HomeScreen = () => {
  const [products, setProducts] = useState([]);

  useEffect(()=> {
    const fetchProducts = async () => {
      const {data} = await axios.get('/api/products');
      setProducts(data);
    }
    fetchProducts();
  }, []);

  return (
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
  )
}

export default HomeScreen;
```

**ProductScreen.js** - \frontend\src\screens\ProductScreen.js <br/>
fetch product details from the server(backend) with the help of axios
```
import React from 'react'
import { useState, useEffect } from 'react';
import { useParams } from 'react-router-dom'
import { Link } from 'react-router-dom';
import {Row, Col, Image, ListGroup, Card, Button} from 'react-bootstrap';
import Rating from '../components/Rating';
import axios from 'axios';

const ProductScreen = () => {
  const [product, setProduct] = useState(0);

  const {id:productId} = useParams();
  
  useEffect(() => {
    const fetchProduct = async () => {
      const {data} = await axios.get(`/api/products/${productId}`);
      setProduct(data);
    }
    fetchProduct();
  }, [productId]);

  return (
    <>
      <Link className='btn btn-light my-3' to='/'>
        Go Back
      </Link>
      <Row>
        <Col md={5}>
          <Image src={product.image} alt={product.name} fluid />
        </Col>
        <Col md={4}>
          <ListGroup variant='flush'>
            <ListGroup.Item>
              <h3>{product.name}</h3>
            </ListGroup.Item>
            <ListGroup.Item>
              <Rating value={product.rating} text={`${product.numReviews} reviews`} />
            </ListGroup.Item>
            <ListGroup.Item>Price: ${product.price}</ListGroup.Item>
            <ListGroup.Item>Description: ${product.description}</ListGroup.Item>
          </ListGroup>
        </Col>
        <Col md={3}>
          <Card>
            <ListGroup variant='flush'>
              <ListGroup.Item>
                <Row>
                  <Col>Price:</Col>
                  <Col>
                    <strong>${product.price}</strong>
                  </Col>
                </Row>
              </ListGroup.Item>
              <ListGroup.Item>
                <Row>
                  <Col>Status:</Col>
                  <Col>
                    <strong>${product.countInStock > 0 ? 'In Stock' : 'Out of Stock'}</strong>
                  </Col>
                </Row>
              </ListGroup.Item>
              <ListGroup.Item>
                <Button className='btn-block' type='button' disabled={product.countInStock === 0}>
                  Add to Cart
                </Button>
              </ListGroup.Item>
            </ListGroup>
          </Card>
        </Col>
      </Row>
    </>
  )
}

export default ProductScreen
```

everything is working fine, so delete the products.js (JSON data) file from frontend

# Starting MongoDB & Mongoose

**login to mongodb.com** <br/>
create new project "**UGshop**" <br/>
**build database** (select free with AWS) <br/>
update the username and password <br/>
automatically set the IP address and all - click on button **finish and close** 

click on connect <br/>
Connecting with MongoDB Driver <br/>
**mongodb+srv://nandishug:<password>@cluster0.wqdxcv4.mongodb.net/?retryWrites=true&w=majority**

**Connect with Mongoose** <br/>
install - **npm i mongoose** <br/>

### Connect with Mongoose
**.env file** - update MONGO connection URL
```
NODE_ENV=development
PORT=5000
MONGO_URI=mongodb+srv://nandishug:*********@cluster0.f5f5ncr.mongodb.net/MERN?retryWrites=true&w=majority
```

create new **config** folder under backend
create new file **db.js** under config 
```
import mongoose from "mongoose";
const connectDB = async() => {
    try {
        const conn = await mongoose.connect(process.env.MONGO_URI);
        console.log(`MongoDB Connected: ${conn.connection.host}`);
    } catch (error) {
        console.log(`Error: ${error.message}`);
        process.exit(1);
    }
};
export default connectDB;
```

### Modeling Our Data
Create new folder models under backend

**userModels.js** <br/>
```
import mongoose from "mongoose";
const userSchema = new mongoose.Schema ({
    name: {
        type: String,
        required: true,
    },
    email: {
        type: String,
        required: true,
        unique: true,
    },
    password: {
        type: String,
        required: true,
    },
    isAdmin: {
        type: Boolean,
        required: true,
        default: false,
    }
}, {
    timestamps: true,
});
const User = mongoose.model("User", userSchema);
export default User;
```

**productModel.js** <br/>
```
import mongoose from "mongoose";
const reviewSchema = mongoose.Schema({
    user: {
        type: mongoose.Schema.Types.ObjectId,
        required: true,
        ref: "User",
    },
    name: {
        type: String,
        required: true,
    },
    rating: {
        type: Number,
        required: true,
    },
    Comment: {
        type: String,
        required: true,
    },
}, {
    timestamps: true,
});
const productSchema = new mongoose.Schema ({
    user: {
        type: mongoose.Schema.Types.ObjectId,
        required: true,
        ref: "User",
    },
    name: {
        type: String,
        required: true,
    },
    image: {
        type: String, 
        required: true,
    },
    brand: {
        type: String,
        required: true,
    },
    category: {
        type: String,
        required: true,
    },
    description: {
        type: String,
        required: true,
    },
    reviews: [reviewSchema],
    rating: {
        type: Number,
        required: true,
        default: 0,
    },
    numReviews: {
        type: Number,
        required: true,
        default: 0,
    },
    price: {
        type: Number,
        required: true,
        default: 0,
    },
    countInStock: {
        type: Number,
        required: true,
        default: 0,
    }
}, {
    timestamps: true,
});
const Product = mongoose.model("Product", productSchema);
export default Product;
```

**orderModel.js** <br />
```
import mongoose from "mongoose";
const orderSchema = mongoose.Schema ({
    user: {
        type: mongoose.Schema.Types.ObjectId,
        required: true,
        ref: "User",
    },
    orderItems: [
        {
            name: {type: String, required: true},
            qty: {type: Number, required: true},
            image: {type: String, required: true},
            price: {type: String, required: true},
            product: {
                type: mongoose.Schema.Types.ObjectId,
                required: true,
                ref: "Product",
            },
        }
    ],
    shippingAddress: {
        address: {type: String, required: true},
        city: {type: String, required: true},
        postalCode: {type: String, required: true},
        country: {type: String, required: true},
    },
    paymentMethod: {
        type: String,
        required: true,
    },
    paymentResult: {
        id: {type: String},
        status: {type: String},
        update_time: {type: String},
        email_address: {type: String},
    },
    itemsPrice: {
        type: Number,
        required: true,
        default: 0.0,
    },
    shippingPrice: {
        type: Number,
        required: true,
        default: 0.0,
    },
    totalPrice: {
        type: Number,
        required: true,
        default: 0.0,
    },
    isPaid: {
        type: Boolean,
        required: true,
        default: false,
    },
    paidAt: {
        type: Date,
    },
    isDelivered: {
        type: Boolean,
        required: true,
        default: false,
    },
    deliveredAt: {
        type: Date,
    }
}, {
    timestamps: true,
});
const Order = mongoose.model('Order', orderSchema);
export default Order;
```

### Prepare sample Data
user password is encrypted - install bcryptjs <br/>
**npm i bcryptjs** <br/>
create new file **users.js** file under backend/data <br/>
```
import bcrypt from 'bcryptjs';
const users = [
    {
        name: 'Admin User',
        email: 'admin@email.com',
        password: bcrypt.hashSync('123456', 10),
        isAdmin: true,
    },
    {
        name: 'John Doe',
        email: 'john@email.com',
        password: bcrypt.hashSync('123456', 10),
        isAdmin: false,
    },
    {
        name: 'Jane Doe',
        email: 'jane@email.com',
        password: bcrypt.hashSync('123456', 10),
        isAdmin: false,
    }
];
export default users;
```

and **products.js** - remove id's because mongo creating unique Id for each object

### Seeding Sample Data
install optional packages <br/>
**npm i colors** <br/>

create new file **seeder.js** file under backend folder <br/>
```
console.log(process.argv)
```

run cmd terminal from roor folder - **node backend/seeder -d**

```
import mongoose from "mongoose";
import dotenv from "dotenv";
import colors from "colors";
import users from "./data/users.js";
import products from "./data/products.js";
import User from "./models/userModel.js";
import Product from "./models/productModel.js";
import Order from "./models/orderModel.js";
import connectDB from "./config/db.js";

dotenv.config();

connectDB();

const importData = async () => {
    try {
        await Order.deleteMany();
        await Product.deleteMany();
        await User.deleteMany();

        const createdUsers = await User.insertMany(users);

        const adminUser = createdUsers[0]._id;

        const sampleProducts = products.map((product) => {
            return {...product, user: adminUser };
        });

        await Product.insertMany(sampleProducts);
        console.log('Data Imported!'.green.inverse);
        process.exit();
    } catch(error) {
        console.error(`${error}`.red.inverse);
        process.exit(1);
    }
}

const destoryData = async () => {
    try {
        await Order.deleteMany();
        await Product.deleteMany();
        await User.deleteMany();

        console.log('Data Destoryed'.green.inverse);
        process.exit();
    } catch(error) {
        console.error(`${error}`.red.inverse);
        process.exit(1);
    }    
};

if(process.argv[2] === '-d') {
    destoryData();
} else {
    importData();
}
```

package.json 
```
scripts: {
  ....
  "data:import": "node backend/seeder.js",
  "data:destory": "node backend/seeder.js -d"
}
```

run terminal - npm run data:import <br/>
check in mongo compass data's are available <br/>
run terminal - npm run data:destory <br/>
check in mongo compass data's are deleted

### Get Products from Database
Create new folder "**routes**" under backend folder <br/>
Create new file "**productRoutes.js**" under routes folder <br/>

create new folder "**middleware**" under backend folder <br/>
create new file "**asyncHandler.js**" under middleware folder <br/>
**asyncHandler.js**
```
const asyncHandler = fn => (req, res, next) => {
  Promise.resolve(fn(req, res, next)).catch(next);
}
export default asyncHandler;
```

**productRoutes.js**
```
import express from "express";
const router = express.Router();
import asyncHandler from "../middleware/asyncHandler.js";
import Product from "../models/productModel.js";

router.get('/', asyncHandler (async (req, res) => {
    const products = await Product.find({});
    res.json(products);
}));

router.get('/:id', asyncHandler(async (req, res) => {
    const product = await Product.findById(req.params.id);
    if(product) {
        return res.json(product);
    } else {
        res.status(404).json({message: 'Product not found'});
    }
}));

export default router;
```

remove below lines from server.js and update in routes/productRoutes.js <br/>
**server.js**
```
app.get('/api/products', (req, res) => {
    res.json(products);
});

app.get('/api/products/:id', (req, res) => {
    const product = products.find((p) => p._id === req.params.id);
    res.json(product);
});
```

```
import express  from "express";
import dotenv from "dotenv";
dotenv.config();
import connectDB from "./config/db.js";
import productRoutes from "./routes/productRoutes.js";

const port = process.env.PORT || 5000;
connectDB();
const app = express();

app.get('/', (req, res) => {
    res.send('API is running...')
});

app.use('/api/products', productRoutes);

app.listen(port, () => console.log(`server running on port ${port}`));
```

### Custom Error Middleware
in Express default error handler <br/>
**https://expressjs.com/en/guide/error-handling.html**

create new file **errorMiddleware.js** under backend/middleware folder <br/>
**errorMiddleware.js**
```
const notFound = (req, res, next) => {
    const error = new Error(`Not Found - ${req.originalUrl}`);
    res.status(404);
    next(error);
};

const errorHandler = (err, req, res, next) => {
    let statusCode = res.statusCode === 200 ? 500 : res.statusCode;
    let message = err.message;

    //check for mongoose bad objectId
    if(err.name === 'CastError' && err.kind === 'ObjectId') {
        message = `Resource not found`;
        statusCode = 404;
    }

    res.status(statusCode).json({
        message,
        stack: process.env.NODE_ENV === 'production' ? '*' : err.stack,
    });
};

export {notFound, errorHandler};
```

update in **server.js** <br/>
**server.js**
```
....
import { notFound, errorHandler } from "./middleware/errorMiddleware.js";
....
app.use(notFound);
app.use(errorHandler);
....
```

and update in **productRoutes.js** <br/>
**productRoutes.js**
```
....
router.get('/:id', asyncHandler(async (req, res) => {
    const product = await Product.findById(req.params.id);
    if(product) {
        return res.json(product);
    } else {
        res.status(404);
        throw new Error('Resource not found');
    }
}));
....
```

### Product Controller
create new folder **controllers** under backend <br/>
create new file **productController.js** under controllers <br/>
**productController.js**
```
import asyncHandler from "../middleware/asyncHandler.js";
import Product from "../models/productModel.js";

//@desc Fetch all products
//@route GET /api/products
//@access Public
const getProducts = asyncHandler(async (req, res) => {
    const products = await Product.find({});
    res.json(products);
});

//@desc Fetch a products
//@route GET /api/products/:id
//@access Public
const getProductById = asyncHandler(async (req, res) => {
    const product = await Product.findById(req.params.id);
    if(product) {
        return res.json(product);
    } else {
        res.status(404);
        throw new Error('Resource not found');
    }
});

export {getProducts, getProductById};
```

and update in **productRoutes.js** <br/>
**productRoutes.js**
```
import express, { Router } from "express";
const router = express.Router();
import { getProducts, getProductById } from "../controllers/productController.js";

router.route('/').get(getProducts);
router.route('/:id').get(getProductById);

export default router;
```

## Redux Toolkit Setup & State Management
### Redux & state overview

### Redux Store & API Slice
Redux - https://redux.js.org/ <br/>
Redux Toolkit - https://redux-toolkit.js.org/ <br/>

install - **npm i @reduxjs/toolkit react-redux** 

create new file **store.js** under frontend/src folder <br/>
```
import {configureStore} from '@reduxjs/toolkit';
const store = configureStore({
    reducer: {}
});
export default store;
```

update in **index.js**
```
....
import { Provider } from 'react-redux';
import store from './store.js';
....
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={store} >
      <RouterProvider router={router} />
    </Provider>
  </React.StrictMode>
);
....
```

create new file **constants.js** under frontend/src folder <br/>
**constants.js**
```
export const BASE_URL = process.env.NODE_ENV === 'development' ? 'http://localhost:5000':'';
export const PRODUCTS_URL = '/api/products';
export const USERS_URL = '/api/users';
export const ORDERS_URL = '/api/orders';
export const PAYPAL_URL = '/api/config/paypal';
```

Slice concept in redux-toolkit way to organize state

create new folder **slices** under frontend/src <br/>
create new file **apiSlice.js** under slices <br/>
**apiSlice.js**
```
import {createApi, fetchBaseQuery} from '@reduxjs/toolkit/query/react';
import { BASE_URL } from '../constants';

const baseQuery = fetchBaseQuery({baseUrl:BASE_URL});

export const apiSlice = createApi({
    baseQuery,
    tagTypes: ['Product', 'Order', 'User'],
    endpoints: (builder) => ({}),
});
```

update in **store.js** 
```
import {configureStore, getDefaultMiddleware} from '@reduxjs/toolkit';
import { apiSlice } from './slices/apiSlice';

const store = configureStore({
    reducer: {
        [apiSlice.reducerPath]: apiSlice.reducer,
    },
    middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(apiSlice.middleware),
    devTools: true,
});

export default store;
```

### products API Slice & Get Products EndPoint
**constants.js** remove the BASE_URL <br/>
export const BASE_URL = '';

create new file **productsApiSlice.js** under slice folder
**productsApiSlice.js**
```
import { PRODUCTS_URL } from "../constants";
import { apiSlice } from "./apiSlice";
export const productsApiSlice = apiSlice.injectEndpoints({
    endpoints: (builder) => ({
        getProducts: builder.query({
            query:() => ({
                url: PRODUCTS_URL,
            }),
            keepUnusedDataFor: 5,
        }),
    }),
})
export const {useGetProductsQuery} = productsApiSlice;
```

update in **HomeScreen.js** <br/>
remove axios, useEffect, useState 
**HomeScreen.js**
```
import React from 'react';
import {Row, Col} from 'react-bootstrap';
import Product from '../components/Product';
import { useGetProductsQuery } from '../slices/productsApiSlice';

const HomeScreen = () => {
  const {data:products, isLoading, error} = useGetProductsQuery();
  return (
    <>
      {isLoading? (<h2>Loading...</h2>) :
      error? (<div>{error?.data?.message || error.error}</div>) : (
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
      )
      }
    </>
  )
}
export default HomeScreen;
```

### Get Product Details EndPoint Challenge 
**productsApiSlice.js**
```
import { PRODUCTS_URL } from "../constants";
import { apiSlice } from "./apiSlice";
export const productsApiSlice = apiSlice.injectEndpoints({
    endpoints: (builder) => ({
        getProducts: builder.query({
            query:() => ({
                url: PRODUCTS_URL,
            }),
            keepUnusedDataFor: 5,
        }),
        getProductDetails: builder.query({
            query:(productId) => ({
                url: `${PRODUCTS_URL}/${productId}`,
            }),
            keepUnusedDataFor: 5,
        }),
    }),
})
export const {useGetProductsQuery, useGetProductDetailsQuery} = productsApiSlice;
```

update in **ProductScreen.js** <br/>
remove useState, useEffect, axios <br/>
**ProductScreen.js**
```
import React from 'react';
import { useParams } from 'react-router-dom'
import { Link } from 'react-router-dom';
import {Row, Col, Image, ListGroup, Card, Button} from 'react-bootstrap';
import Rating from '../components/Rating';
import { useGetProductDetailsQuery } from '../slices/productsApiSlice';

const ProductScreen = () => {
  const {id:productId} = useParams();
  const {data:product, isLoading, error} = useGetProductDetailsQuery(productId);

  return (
    <>
      <Link className='btn btn-light my-3' to='/'>
        Go Back
      </Link>
      {isLoading ? (
        <h2>Loading...</h2>
      ) : error? (
        <div>{error?.data?.message || error.error}</div>
      ) : (
        <Row>
        <Col md={5}>
          <Image src={product.image} alt={product.name} fluid />
        </Col>
        <Col md={4}>
          <ListGroup variant='flush'>
            <ListGroup.Item>
              <h3>{product.name}</h3>
            </ListGroup.Item>
            <ListGroup.Item>
              <Rating value={product.rating} text={`${product.numReviews} reviews`} />
            </ListGroup.Item>
            <ListGroup.Item>Price: ${product.price}</ListGroup.Item>
            <ListGroup.Item>Description: ${product.description}</ListGroup.Item>
          </ListGroup>
        </Col>
        <Col md={3}>
          <Card>
            <ListGroup variant='flush'>
              <ListGroup.Item>
                <Row>
                  <Col>Price:</Col>
                  <Col>
                    <strong>${product.price}</strong>
                  </Col>
                </Row>
              </ListGroup.Item>
              <ListGroup.Item>
                <Row>
                  <Col>Status:</Col>
                  <Col>
                    <strong>${product.countInStock > 0 ? 'In Stock' : 'Out of Stock'}</strong>
                  </Col>
                </Row>
              </ListGroup.Item>
              <ListGroup.Item>
                <Button className='btn-block' type='button' disabled={product.countInStock === 0}>
                  Add to Cart
                </Button>
              </ListGroup.Item>
            </ListGroup>
          </Card>
        </Col>
      </Row>
      )}

    </>
  )
}

export default ProductScreen
```

### Loader & Message Components
create new file **Loader.js** under frontend/src/components <br/>
**Loader.js**
```
import { Spinner } from "react-bootstrap";
const Loader = () => {
    return (
        <Spinner 
            animation="border"
            role="status"
            style={{
                width:'100px',
                height:'100px',
                margin:'auto',
                display:'block',
            }} > </Spinner>
    );
}
export default Loader;
```

create new file **Message.js** under frontend/src/components <br/>
**Message.js**
```
import { Alert } from "react-bootstrap";
const Message = ({variant, children}) => {
    return (
        <Alert variant={variant}>
            {children}
        </Alert>
    )
}
Message.defaultProps = {
    variant: 'info',
}
export default Message;
```

update in **HomeScreen.js** and **ProductScreen.js** <br/>
**HomeScreen.js**
```
....
import Loader from '../components/Loader.js';
import Message from '../components/Message';
....
{isLoading? (<Loader />) :
      error? (
      <Message variant='danger'>{error?.data?.message || error.error}</Message>) : (
        <>
          <h1>Latest Products</h1>
....
```

**ProductScreen.js**
```
....
import Loader from '../components/Loader.js';
import Message from '../components/Message';
....
{isLoading ? (
        <Loader />
      ) : error? (
        <Message variant='danger'>{error?.data?.message || error.error}</Message>
      ) : (
        <Row>
....
```

## Shopping Cart Functionality
### Cart Slice & Reducer
create new file **cartSlice.js** under frontent/src/slices <br/>
**cartSlice.js**
```
import { createSlice } from "@reduxjs/toolkit";
const initialState = localStorage.getItem("cart") ? JSON.parse(localStorage.getItem("cart")) : {cartItems: []};
const cartSlice = createSlice({
  name: "cart",
  initialState,
  reducers: {}
});
export default cartSlice.reducer;
```

update in **store.js** <br/>
**store.js**
```
import {configureStore} from '@reduxjs/toolkit';
import { apiSlice } from './slices/apiSlice.js';
import cartSliceReducer from './slices/cartSlice.js';

const store = configureStore({
    reducer: {
        [apiSlice.reducerPath]: apiSlice.reducer,
        cart: cartSliceReducer,
    },
    middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(apiSlice.middleware),
    devTools: true,
});

export default store;
```

### Add to Cart Function
Modify/update the **cartSlice.js** 
```
import { createSlice } from "@reduxjs/toolkit";

const initialState = localStorage.getItem("cart") ? JSON.parse(localStorage.getItem("cart")) : {cartItems: []};

const addDecimals = (num) => {
  return (Math.round(num*100)/100).toFixed(2);
}

const cartSlice = createSlice({
  name: "cart",
  initialState,
  reducers: {
    addToCart: (state, action) => {
      const item = action.payload;
      const existItem = state.cartItems.find((x) => x._id === item._id);

      if(existItem) {
        state.cartItems = state.cartItems.map((x) => x._id === existItem._id ? item : x);
      } else {
        state.cartItems = [...state.cartItems, item];
      }

      //calculate items price
      state.itemsPrice = addDecimals(state.cartItems.reduce((acc, item) => acc+item.price * item.qty, 0));

      //calculate shipping price (if order is over $100 then free, else $10 shipping)
      state.shippingPrice = addDecimals(state.itemsPrice > 100 ? 0 : 10);

      //calculate tax price (15% tax)
      state.taxPrice = addDecimals(Number((0.15 * state.itemsPrice).toFixed(2)));

      //calculate total price
      state.totalPrice = (Number(state.itemsPrice) + Number(state.shippingPrice) + Number(state.taxPrice)).toFixed(2);

      localStorage.setItem('cart', JSON.stringify(state));
    }
  }
});

export const {addToCart} = cartSlice.actions;

export default cartSlice.reducer;
```

### Qty & Add to Cart Handler
**productScreen.jsx** modify the below changes

```
import React, { useState } from 'react';
import { useParams, useNavigate } from 'react-router-dom';
import { Link } from 'react-router-dom';
import {Form, Row, Col, Image, ListGroup, Card, Button} from 'react-bootstrap';
import { useDispatch } from 'react-redux';
import Rating from '../components/Rating';
import Loader from '../components/Loader.js';
import Message from '../components/Message';
import { useGetProductDetailsQuery } from '../slices/productsApiSlice';
import { addToCart } from '../slices/cartSlice';

const ProductScreen = () => {
  const {id:productId} = useParams();
  const dispatch = useDispatch();
  const navigate = useNavigate();
  const [qty, setQty] = useState(1);

  const {data:product, isLoading, error} = useGetProductDetailsQuery(productId);

  const addToCartHandler = () => {
    dispatch(addToCart({...product, qty}));
    navigate('/cart');
  };

  return (
    <>
      <Link className='btn btn-light my-3' to='/'>
        Go Back
      </Link>
      {isLoading ? (
        <Loader />
      ) : error? (
        <Message variant='danger'>{error?.data?.message || error.error}</Message>
      ) : (
        <Row>
        <Col md={5}>
          <Image src={product.image} alt={product.name} fluid />
        </Col>
        <Col md={4}>
          <ListGroup variant='flush'>
            <ListGroup.Item>
              <h3>{product.name}</h3>
            </ListGroup.Item>
            <ListGroup.Item>
              <Rating value={product.rating} text={`${product.numReviews} reviews`} />
            </ListGroup.Item>
            <ListGroup.Item>Price: ${product.price}</ListGroup.Item>
            <ListGroup.Item>Description: ${product.description}</ListGroup.Item>
          </ListGroup>
        </Col>
        <Col md={3}>
          <Card>
            <ListGroup variant='flush'>
              <ListGroup.Item>
                <Row>
                  <Col>Price:</Col>
                  <Col>
                    <strong>${product.price}</strong>
                  </Col>
                </Row>
              </ListGroup.Item>
              <ListGroup.Item>
                <Row>
                  <Col>Status:</Col>
                  <Col>
                    <strong>${product.countInStock > 0 ? 'In Stock' : 'Out of Stock'}</strong>
                  </Col>
                </Row>
              </ListGroup.Item>

              {product.countInStock > 0 && (
                <ListGroup.Item>
                  <Row>
                    <Col>Qty</Col>
                    <Col>
                      <Form.Control 
                        as='select' 
                        value={qty} 
                        onChange={(e) => setQty(Number(e.target.value))}> 
                          {[...Array(product.countInStock).keys()].map((x) => (
                            <option key = {x+1} value={x+1} >
                              {x + 1}
                            </option>
                          )) }
                      </Form.Control>
                    </Col>
                  </Row>
                </ListGroup.Item>
              )}
              <ListGroup.Item>
                <Button className='btn-block' 
                  type='button' 
                  disabled={product.countInStock === 0} 
                  onClick={addToCartHandler}>
                  Add to Cart
                </Button>
              </ListGroup.Item>
            </ListGroup>
          </Card>
        </Col>
      </Row>
      )}

    </>
  )
}

export default ProductScreen
```

### Cart Utils File
create new folder and file under frontend/src <br/>
frontend/src -> utils/cartUtils.js <br/>
**cartUtils.js** <br/>
```
export const addDecimals = (num) => {
    return (Math.round(num*100)/100).toFixed(2);
};
export const updateCart =(state) => {
    //calculate items price
    state.itemsPrice = addDecimals(state.cartItems.reduce((acc, item) => acc+item.price * item.qty, 0));

    //calculate shipping price (if order is over $100 then free, else $10 shipping)
    state.shippingPrice = addDecimals(state.itemsPrice > 100 ? 0 : 10);

    //calculate tax price (15% tax)
    state.taxPrice = addDecimals(Number((0.15 * state.itemsPrice).toFixed(2)));

    //calculate total price
    state.totalPrice = (Number(state.itemsPrice) + Number(state.shippingPrice) + Number(state.taxPrice)).toFixed(2);

    localStorage.setItem('cart', JSON.stringify(state));

    return state;
}
```

modify the cartSlice.js <br/>
```
import { createSlice } from "@reduxjs/toolkit";
import {updateCart} from '../utils/cartUtils';

const initialState = localStorage.getItem('cart') ? JSON.parse(localStorage.getItem('cart')) : {cartItems: []};

const cartSlice = createSlice({
  name: 'cart',
  initialState,
  reducers: {
    addToCart: (state, action) => {
      const item = action.payload;
      const existItem = state.cartItems.find((x) => x._id === item._id);

      if(existItem) {
        state.cartItems = state.cartItems.map((x) => x._id === existItem._id ? item : x);
      } else {
        state.cartItems = [...state.cartItems, item];
      }
      
      return updateCart(state);
    }
  }
});

export const {addToCart} = cartSlice.actions;

export default cartSlice.reducer;
```

### Item Count in Header
modify the Header.js - adding cart number in navbar <br/>
**Header.js**
```
import react from 'react'
import {Badge, Navbar, Nav, Container} from 'react-bootstrap';
import {FaShoppingCart, FaUser} from 'react-icons/fa';
import {LinkContainer} from 'react-router-bootstrap';
import {useSelector} from 'react-redux';

const Header = () => {
  const {cartItems} = useSelector((state) => state.cart);

  return (
    <header>
      <Navbar bg='dark' variant='dark' expand='md' collapseOnSelect>
        <Container>
          <LinkContainer to='/'>
            <Navbar.Brand>ProShop</Navbar.Brand>
          </LinkContainer>
          <Navbar.Toggle aria-controls='basic-navbar-nav' />
          <Navbar.Collapse id='basic-navbar-nav'>
            <Nav className='ms-auto'>
              <LinkContainer to='/cart'>
                <Nav.Link >
                  <FaShoppingCart /> Cart
                  {cartItems.length > 0 && (
                    <Badge pill bg='success' style = {{marginleft:'5px'}}>
                      {cartItems.reduce((a,c) => a + c.qty, 0)}
                    </Badge>                
                  )}
                </Nav.Link>
              </LinkContainer>
              <LinkContainer to='/login'>
                <Nav.Link>
                  <FaUser />Sign In
                </Nav.Link>
              </LinkContainer>
            </Nav>
          </Navbar.Collapse>
        </Container>
      </Navbar>
    </header>
  )
}

export default Header;
```

### Cart Screen
create new file **CartScreen.js** under screen folder <br/>
**CartScreen.js**
```
import React from 'react';
import {Link, useNavigate} from 'react-router-dom';
import { useDispatch, useSelector } from 'react-redux';
import {Row, Col, ListGroup, Image, Form, Button, Card} from 'react-bootstrap';
import {FaTarsh} from 'react-icons/fa';
import Message from '../components/Message';

const CartScreen = () => {
    const navigate = useNavigate();
    const dispatch = useDispatch();

    return (
        <div>CartScreen</div>
    )
}

export default CartScreen
```

update in **index.js** file <br/>
**index.js**
```
....
import CartScreen from './screens/CartScreen';
....

<Route  path='/cart' element={<CartScreen />} />
....
```

update all the feilds in **cartScreen.js** <br/>
```
import React from 'react';
import {Link, useNavigate} from 'react-router-dom';
import { useDispatch, useSelector } from 'react-redux';
import {Row, Col, ListGroup, Image, Form, Button, Card} from 'react-bootstrap';
import {FaTarsh, FaTrash} from 'react-icons/fa';
import Message from '../components/Message';
import { addToCart } from '../slices/cartSlice';

const CartScreen = () => {
  const navigate = useNavigate();
  const dispatch = useDispatch();

  const cart = useSelector((state) => state.cart);
  const {cartItems} = cart;

  const addToCartHandler = async(product, qty) => {
    dispatch(addToCart({...product, qty}))
  };
  
  return (
    <Row>
      <Col md={8}>
        <h1 style={{marginBottom:'20px'}}>Shopping Cart</h1>
        {cartItems.length === 0 ? (
          <Message>
            Your cart is empty <Link to ='/'>Go Back</Link>
          </Message>
        ) : (
          <ListGroup variant='flush'>
            {cartItems.map((item) => (
              <ListGroup.Item key={item._id}>
                <Row>
                  <Col md={2}>
                    <Image src={item.image} alt={item.name} fluid rounded />
                  </Col>
                  <Col md={3}>
                    <Link to={`/product/${item._id}`}>{item.name}</Link>
                  </Col>
                  <Col md={2}>${item.price}</Col>
                  <Col md={2}>
                    <Form.Control
                      as='select'
                      value={item.qty}
                      onChange={(e) => addToCartHandler(item, Number(e.target.value))}>
                      {[...Array(item.countInStock).keys()].map((x) => (
                        <option key={x + 1} value={x + 1}>
                          {x + 1}
                        </option>
                      ))}
                    </Form.Control>
                  </Col>
                  <Col md={2}>
                    <Button type='button' variant='light'>
                      <FaTrash />
                    </Button>
                  </Col>
                </Row>
              </ListGroup.Item>
            ))}
          </ListGroup>
        )}
      </Col>
      <Col md={4}>
        <Card>
          <ListGroup variant='flush'>
            <ListGroup.Item>
              <h2>
                Subtotal ({cartItems.reduce((acc, item) => acc + item.qty, 0)}) items
              </h2>
              ${cartItems.reduce((acc, item) => acc + item.qty * item.price, 0).toFixed(2)}
            </ListGroup.Item>
            <ListGroup.Item>
              <Button type='button' className='btn-block' disabled={cartItems.length === 0}>
                Proceed To Checkout
              </Button>
            </ListGroup.Item>
          </ListGroup>
        </Card>
      </Col>
    </Row>
  )
}

export default CartScreen
```

### Remove from Cart
add removeFromCart function in cartSlice.js <br/>
```
.....
.....
  
      return updateCart(state);
    },
    removeFromCart:(state, action) => {
      state.cartItems = state.cartItems.filter((x) => x._id !== action.payload);
      return updateCart(state);
    }
  }
});

export const {addToCart, removeFromCart} = cartSlice.actions;

export default cartSlice.reducer;
```

and update in **cartScreen.js** <br/>
```
....
import { addToCart, removeFromCart } from '../slices/cartSlice';
....
....
const removeFromCartHandler = async(id) => {
    dispatch(removeFromCart(id));
  };
....
<Col md={2}>
  <Button type='button' variant='light' onClick={() => removeFromCartHandler(item._id)}>
    <FaTrash />
  </Button>
</Col>
```

**"Proceed to checkout"** button <br/>
```
....
const checkoutHandler = () => {
    navigate('/login?redirect=/shipping');
  };
....
<ListGroup.Item>
  <Button type='button' className='btn-block' disabled={cartItems.length === 0} onClick={checkoutHandler}>
    Proceed To Checkout
  </Button>
</ListGroup.Item>
....
```

## Backend Authentication
### User Routes & Controller
create new file "**userRoutes.js**" under backend/routes <br/>
create new file "**userController.js**" under backend/controllers <br/>

**userController.js**
```
import asyncHandler from '../middleware/asyncHandler.js';
import User from '../models/userModel.js';

//@desc Auth user & get token
//@route POST /api/users/login
//@access Public
const authUser = asyncHandler(async (req, res) => {
    res.send('auth user');
});

//@desc Register User
//@route POST /api/users
//@access Public
const registerUser = asyncHandler(async (req, res) => {
    res.send('register user');
});

//@desc Logout User/clear cookie
//@route POST /api/users/logout
//@access Private
const logoutUser = asyncHandler(async (req, res) => {
    res.send('logout user');
});

//@desc Get User profile
//@route GET /api/users/profile
//@access Private
const getUserProfile = asyncHandler(async (req, res) => {
    res.send('get user profile');
});

//@desc update User profile
//@route PUT /api/users/profile
//@access Private
const updateUserProfile = asyncHandler(async (req, res) => {
    res.send('update user profile');
});

//@desc Get users
//@route GET /api/users
//@access Private/Admin
const getUsers = asyncHandler(async (req, res) => {
    res.send('get users');
});

//@desc Get user by ID
//@route GET /api/users/:id
//@access Private/Admin
const getUserByID = asyncHandler(async (req, res) => {
    res.send('get users by id');
});

//@desc Delete users
//@route DELETE /api/users/:id
//@access Private/Admin
const deleteUser = asyncHandler(async (req, res) => {
    res.send('delete user');
});

//@desc Update user
//@route PUT /api/users/:id
//@access Private/Admin
const updateUser = asyncHandler(async (req, res) => {
    res.send('update user');
});

export {
    authUser,
    registerUser,
    logoutUser,
    getUserProfile,
    updateUserProfile,
    getUsers,
    deleteUser,
    getUserByID,
    updateUser
};
```

**userRoutes.js**
```
import express from 'express';
const router = express.Router();

import {
    authUser,
    registerUser,
    logoutUser,
    getUserProfile,
    updateUserProfile,
    getUsers,
    deleteUser,
    getUserByID,
    updateUser
} from '../controllers/userController.js';

router.route('/').post(registerUser).get(getUsers);
// router.post('/logout', logoutUser);
// router.post('/login', authUser);
// router.route('/profile').get(getUserProfile).put(updateUserProfile);
// router.route('/:id').delete(deleteUser).get(getUserByID).put(updateUser);

export default router;
```

and update in **server.js** <br/>
```
import userRoutes from './routes/userRoutes.js';
....
....
app.use('/api/users', userRoutes);
....
```

### User Email & Password Validation
**server.js**
```
//Body parser middleware
app.use(express.json());
app.use(express.urlencoded({extended:true}));
```

**userController.js** <br/>
```
const authUser = asyncHandler(async (req, res) => {
    const {email, password} = req.body;
    const user = await User.findOne({email});
    if(user) {
        res.json({
            _id:user._id,
            name:user.name,
            email:user.email,
            isAdmin:user.isAdmin
        });
    } else {
        res.status(401);
        throw new Error('Invalid email or password');
    }
    res.send('auth user');
});
```

**userModel.js** <br/>
```
import bcrypt from 'bcryptjs';
....
userSchema.methods.matchPassword = async function(enteredPassword) {
    return await bcrypt.compare(enteredPassword, this.password);
};
```

**userController.js** <br/>
```
 if(user && (await user.matchPassword(password))) {
        res.json({
            _id:user._id,
            name:user.name,
            email:user.email,
            isAdmin:user.isAdmin
        });
    }
```

### How Do JSON Web Tokens Work? JWT HTTP only Cookie
install JWT in main root folder <br/>
**npm i jsonwebtoken** <br/>
**userController.js**
```
import jwt from 'jsonwebtoken';
...
   if(user && (await user.matchPassword(password))) {
        const token = jwt.sign({userId:user._id}, process.env.JWT_SECRET, {
            expiresIn:'30d',
        });
        //set JWT as HTTP-only cookie
        res.cookie('jwt', token, {
            httpOnly: true,
            secure: process.env.NODE_ENV !== 'development',
            sameSite: 'strict',
            maxAge: 30 * 24 * 60 * 60 * 1000 //30days
        })
...
```

**.env file** <br/>
JWT_SECRET = abc123

**.example_env** <br/>
JWT_SECRET = ADD_YOUR_SECRET

### Auth Middleware & Endpoint
install **cookie-parser** in root folder <br/>
**npm i cookie-parser** <br/>

**server.js** <br/>
```
import cookieParser from "cookie-parser";
....
....
//Body parser middleware
app.use(express.json());
app.use(express.urlencoded({extended:true}));
....
//cookie parser middleware
app.use(cookieParser());
```

create new file under middleware folder <br />
**authMiddleware.js**
```
import jwt from 'jsonwebtoken';
import asyncHandler from './asyncHandler';
import User from '../models/userModel';

//protect routes
const protect = asyncHandler(async (req, res, next) => {
    let token;

    //read the JWT from the cookie
    token = req.cookies.jwt;
    if(token) {
        try {
            const decoded = jwt.verify(token, process.env.JWT_SECRET);
            req.user = await User.findById(decoded.userId).select('-password');
            next();
        } catch (error) {
            console.log(error);
            res.status(401);
            throw new Error('Not authorized, token failed');
        }
    } else { 
        res.status(401);
        throw new Error('Not authorized, no token');
    }
});

//Admin middleware
const admin = (req, res, next) => {
    if(req.user && req.user.isAdmin) {
        next();
    } else {
        res.status(401);
        throw new Error('not authorized as admin');
    }
}

export {protect, admin};
```

modify the **userRoutes.js** <br />
```
....
....
import { protect, admin } from '../middleware/authMiddleware.js';

router.route('/').post(registerUser).get(protect, admin, getUsers);
router.post('/logout', logoutUser);
router.post('/auth', authUser);
router.route('/profile').get(protect, getUserProfile).put(protect, updateUserProfile);
router.route('/:id').delete(protect, admin, deleteUser).get(protect, admin, getUserByID).put(protect, admin, updateUser);

export default router;
```

### Logout User & Clear Cookie
modify the below lines in **userController.js** 
```
//@desc Logout User/clear cookie
//@route POST /api/users/logout
//@access Private
const logoutUser = asyncHandler(async (req, res) => {
    res.cookie('jwt', '', {
        httpOnly: true,
        expires: new Date(0)
    });
    res.status(200).json({message:'Logged out successfully'});
});
```

### User Register EndPoint & Encryption
modify the below code in **userController.js** <br/>
```
//@desc Register User
//@route POST /api/users
//@access Public
const registerUser = asyncHandler(async (req, res) => {
    const {name, email, password} = req.body;
    const userExists = await User.findOne({email});
    if(userExists) {
        res.status(400);
        throw new Error('user already exists');
    }
    const user = await User.create({
        name,
        email,
        password
    });
    if(user) {
        res.status(201).json({
            _id: user._id,
            name: user.name,
            email: user.email,
            isAdmin: user.isAdmin
        });
    } else {
        res.status(400);
        throw new Error('Invalid user data');
    }
});
```

and modify the below changes in **userModel.js** <br/>
```
....
....
userSchema.methods.matchPassword = async function(enteredPassword) {
    return await bcrypt.compare(enteredPassword, this.password);
};

userSchema.pre('save', async function(next) {
    if(!this.isModified('password')) {
        next();
    }
    const salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, salt);
});
```

create new folder "**utils**" under backend folder <br/>
create new file "**generateToken.js**" under backend/utils <br/>
**generateToken.js**
```
import jwt from "jsonwebtoken";

const generateToken = (res, userId) => {
    const token = jwt.sign({userId}, process.env.JWT_SECRET, {
        expiresIn: '30d'
    });

    //set JWT as HTTPOnly cookie
    res.cookie('jwt', token, {
        httpOnly: true,
        secure: process.env.NODE_ENV !== 'development',
        sameSite: 'strict',
        maxAge: 30 * 24 * 60 * 60 * 1000 //30days
    });
}

export default generateToken;
```

**userController.js** <br/>
remove - import jwt from 'jwttoken'
```
import generateToken from '../utils/generateToken.js'
....
....
if(user && (await user.matchPassword(password))) {
        generateToken(res, user._id);
        res.json({
            _id:user._id,
            name:user.name,
            email:user.email,
            isAdmin:user.isAdmin
        });
    }
....
....
if(user) {
        generateToken(res, user._id);
        res.status(201).json({
            _id: user._id,
            name: user.name,
            email: user.email,
            isAdmin: user.isAdmin
        });
    }
....
```

### User Profile EndPoints
modify the getUserProfile and updateUserProfile in **userController.js**
```
....
....
//@desc Get User profile
//@route GET /api/users/profile
//@access Private
const getUserProfile = asyncHandler(async (req, res) => {
    const user = await User.findById(req.user._id);
    if(user) {
        res.status(200).json({
            _id: user._id,
            name: user.name,
            email: user.email,
            isAdmin: user.isAdmin
        });
    } else {
        res.status(404);
        throw new Error('user not found');
    }
});
....
....
//@desc update User profile
//@route PUT /api/users/profile
//@access Private
const updateUserProfile = asyncHandler(async (req, res) => {
    const user = await User.findById(req.user._id);
    if(user) {
        user.name = req.body.name || user.name;
        user.email = req.body.email || user.email;
        if(req.body.password) {
            user.password = req.body.password;
        }
        const updatedUser = await user.save();
        res.status(200).json ({
            _id: updatedUser._id,
            name: updateUser.name,
            email: updateUser.email,
            isAdmin: updateUser.isAdmin
        });
    } else {
        res.status(404);
        throw new Error('user not found');
    }
});
....
```

## Frontend Authentication
### Auth & User API Slice
create new file "**authSlice.js**" under frontend/slices <br/>
**authSlice.js**
```
import {createSlice} from '@reduxjs/toolkit';

const initialState = {
    userInfo: localStorage.getItem('userInfo')?JSON.parse(localStorage.getItem('userInfo')):null
}

const authSlice = createSlice ({
    name: 'auth',
    initialState,
    reducers : {
        setCredentials: (state, action) => {
            state.userInfo = action.payload;
            localStorage.setItem('userInfo', JSON.stringify(action.payload));
        }
    }
})

export const {setCredentials} = authSlice.actions;
export default authSlice.reducer;
```

and update in **store.js** <br/>
```
import authSliceReducer from './slices/authSlice.js';
....
....
auth: authSliceReducer
....
```

create new file **usersApiSlice.js** under frontend/slices <br/>
**usersApiSlice.js**
```
import { USERS_URL } from "../constants";
import { apiSlice } from "./apiSlice";

export const usersApiSlice = apiSlice.injectEndpoints ({
    endpoints: (builder) => ({
        login: builder.mutation ({
            query: (data) => ({
                url: `${USERS_URL}/auth`, 
                    method: 'POST',
                    body: data,
            })
        })
    })
})

export const {useLoginMutation} = usersApiSlice;
```

### Login Screen
create new file "**FormContainer.js**" under frontend/components <br/>
**FormContainer.js**
```
import { Container, Row, Col } from "react-bootstrap";
const FormContainer = ({children}) => {
    return (
        <Container>
            <Row className="justify-content-md-container">
                <Col xs={12} md={6}>
                    {children}
                </Col>
            </Row>
        </Container>
    )
}
export default FormContainer;
```

create new file "**LoginScreen.js**" under frontend/screens <br/>
**LoginScreen.js**
```
import { useState } from "react";
import {Link} from "react-router-dom";
import {Form, Button, Row, Col} from 'react-bootstrap';
import FormContainer from "../components/FormContainer";

const LoginScreen = () => {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');

    const submitHandler = (e) => {
        e.preventDefault();
        console.log('submit');
    };

    return (
        <FormContainer>
            <h1>Sign In</h1>
            <Form onSubmit={submitHandler}>
                <Form.Group controlId="email" className="my-3">
                    <Form.Label>Email Address</Form.Label>
                    <Form.Control 
                        type="email"
                        placeholder="Enter email"
                        value={email}
                        onChange={(e) => setEmail(e.target.value)} >
                    </Form.Control>
                </Form.Group>
                <Form.Group controlId="password" className="my-3">
                    <Form.Control 
                        type="password"
                        placeholder="Enter password"
                        value={password}
                        onChange={(e) => setPassword(e.target.value)} >
                    </Form.Control>
                </Form.Group>
                <Button type="submit" variant="primary" className="mt-2">
                    Sign In
                </Button>
            </Form>
            <Row className="py-3">
                <Col>
                    new customer ? <Link to = '/register'> Register </Link>
                </Col>
            </Row>
        </FormContainer>
    )
}

export default LoginScreen;
```

update in **index.js** <br/>
**index.js** 
```
....
import LoginScreen from './screens/LoginScreen.js';

const router = createBrowserRouter (
  createRoutesFromElements (
    <Route path='/' element={<App/>}>
....
      <Route  path='/login' element={<LoginScreen />} />
    </Route>
....
```

### Login Functionality
install **react-toastify** under frontend <br />
**npm i react-toastify** <br />
update in **App.js** <br />
**App.js**
```
import {ToastContainer} from 'react-toastify';
import 'react-toastify/dist/ReactToastify.css';
<ToastContainer />
```

update in **LoginScreen.js** <br />
**LoginScreen.js**
```
import { useState, useEffect } from "react";
import {Link, useLocation, useNavigate} from "react-router-dom";
import {Form, Button, Row, Col} from 'react-bootstrap';
import { useDispatch, useSelector } from "react-redux";
import FormContainer from "../components/FormContainer";
import Loader from "../components/Loader";
import { useLoginMutation } from "../slices/usersApiSlice";
import { setCredentials } from "../slices/authSlice";
import {toast} from 'react-toastify';

const LoginScreen = () => {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');

    const dispatch = useDispatch();
    const navigate = useNavigate();

    const [login, {isLoading}] = useLoginMutation();
    const {userInfo} = useSelector((state) => state.auth);
    const {search} = useLocation();
    const sp = new URLSearchParams(search);
    const redirect = sp.get('redirect') || '/';

    useEffect(() => {
        if(userInfo) {
            navigate(redirect);
        }
    }, [userInfo, redirect, navigate]);

    const submitHandler = async (e) => {
        e.preventDefault();
        try {
            const res = await login({email, password}).unwrap();
            dispatch(setCredentials({...res}));
            navigate(redirect);
        } catch(err) {
            toast.error(err?.data.message || err.error);
        }
    };

    return (
        <FormContainer>
            <h1>Sign In</h1>
            <Form onSubmit={submitHandler}>
                <Form.Group controlId="email" className="my-3">
                    <Form.Label>Email Address</Form.Label>
                    <Form.Control 
                        type="email"
                        placeholder="Enter email"
                        value={email}
                        onChange={(e) => setEmail(e.target.value)} >
                    </Form.Control>
                </Form.Group>
                <Form.Group controlId="password" className="my-3">
                    <Form.Label>Password</Form.Label>
                    <Form.Control 
                        type="password"
                        placeholder="Enter password"
                        value={password}
                        onChange={(e) => setPassword(e.target.value)} >
                    </Form.Control>
                </Form.Group>
                <Button type="submit" variant="primary" className="mt-2">
                    Sign In
                </Button>
                {isLoading && <Loader />}
            </Form>
            <Row className="py-3">
                <Col>
                    new customer ? {''}
                    <Link to = {redirect?`/register?redirect=${redirect}`:'/register'}> Register </Link>
                </Col>
            </Row>
        </FormContainer>
    )
}

export default LoginScreen;
```

update in **Header.js** <br/>
**Header.js**
```
import react from 'react'
import {Badge, Navbar, Nav, Container, NavDropdown} from 'react-bootstrap';
import {FaShoppingCart, FaUser} from 'react-icons/fa';
import {LinkContainer} from 'react-router-bootstrap';
import {useSelector} from 'react-redux';

const Header = () => {
  const {cartItems} = useSelector((state) => state.cart);
  const {userInfo} = useSelector((state) => state.auth);

  const logoutHandler = () => {
    console.log('logout');
  }

  return (
    <header>
      <Navbar bg='dark' variant='dark' expand='md' collapseOnSelect>
        <Container>
          <LinkContainer to='/'>
            <Navbar.Brand>ProShop</Navbar.Brand>
          </LinkContainer>
          <Navbar.Toggle aria-controls='basic-navbar-nav' />
          <Navbar.Collapse id='basic-navbar-nav'>
            <Nav className='ms-auto'>
              <LinkContainer to='/cart'>
                <Nav.Link >
                  <FaShoppingCart /> Cart
                  {cartItems.length > 0 && (
                    <Badge pill bg='success' style = {{marginleft:'5px'}}>
                      {cartItems.reduce((a,c) => a + c.qty, 0)}
                    </Badge>                
                  )}
                </Nav.Link>
              </LinkContainer>
              {userInfo ? (
                <NavDropdown title={userInfo.name} id='username'>
                  <LinkContainer to = '/profile'>
                    <NavDropdown.Item>Profile</NavDropdown.Item>
                  </LinkContainer>
                  <NavDropdown.Item onClick={logoutHandler}>
                    Logout
                  </NavDropdown.Item>
                </NavDropdown>
              ) : (
                <LinkContainer to='/login'>
                <Nav.Link>
                  <FaUser />Sign In
                </Nav.Link>
              </LinkContainer>
              )}
            </Nav>
          </Navbar.Collapse>
        </Container>
      </Navbar>
    </header>
  )
}

export default Header;
```

### User Logout
add logout function into **usersApiSlice.js** <br/>
```
....
logout: builder.mutation ({
            query: () => ({
                url: `${USERS_URL}/logout`, 
                    method: 'POST'
            })
        })

export const {useLoginMutation, useLogoutMutation} = usersApiSlice;
```

add logout function into **authSlice.js** <br/>
```
....
logout: (state, action) => {
            state.userInfo = null;
            localStorage.removeItem('userInfo');
        }
export const {setCredentials, logout} = authSlice.actions;
```

update in **Header.js** <br/>
```
....
import { useNavigate } from 'react-router-dom';
import {useSelector, useDispatch} from 'react-redux';
import { useLoginMutation, useLogoutMutation } from '../slices/usersApiSlice';
import {logout} from '../slices/authSlice';
....
const dispatch = useDispatch();
  const navigate = useNavigate();
  const [logoutApiCall] = useLogoutMutation();
  
  const logoutHandler = async () => {
    try {
      await logoutApiCall().unwrap();
      dispatch(logout());
      navigate('/login');
    } catch(err) {
      console.log(err);
    }
  }
....
```

### User Register
update in **usersApiSlice.js** <br/>
```
register: builder.mutation ({
            query: (data) => ({
                url: `${USERS_URL}`, 
                    method: 'POST',
                    body: data,
            })
        }),

export const {useLoginMutation, useLogoutMutation, useRegisterMutation} = usersApiSlice;
```

create new file "**RegisterScreen.js**" under frontend/screens <br/>
**RegisterScreen.js**
```
import { useState, useEffect } from "react";
import {Link, useLocation, useNavigate} from "react-router-dom";
import {Form, Button, Row, Col} from 'react-bootstrap';
import { useDispatch, useSelector } from "react-redux";
import FormContainer from "../components/FormContainer.js";
import Loader from "../components/Loader";
import { useRegisterMutation } from "../slices/usersApiSlice";
import { setCredentials } from "../slices/authSlice";
import {toast} from 'react-toastify';

const RegisterScreen = () => {
    const [name, setName] = useState('');
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');
    const [confirmPassword, setConfirmPassword] = useState('');

    const dispatch = useDispatch();
    const navigate = useNavigate();

    const [register, {isLoading}] = useRegisterMutation();
    const {userInfo} = useSelector((state) => state.auth);
    const {search} = useLocation();
    const sp = new URLSearchParams(search);
    const redirect = sp.get('redirect') || '/';

    useEffect(() => {
        if(userInfo) {
            navigate(redirect);
        }
    }, [userInfo, redirect, navigate]);

    const submitHandler = async (e) => {
        e.preventDefault();
        if(password !== confirmPassword) {
            toast.error('password does not match');
        } else {
            try {
                const res = await register({name, email, password}).unwrap();
                dispatch(setCredentials({...res}));
                navigate(redirect);
            } catch(err) {
                toast.error(err?.data.message || err.error);
            }
        }
    };

    return (
        <FormContainer>
            <h1>Sign Up</h1>
            <Form onSubmit={submitHandler}>
                <Form.Group controlId="name" className="my-3">
                    <Form.Label>Name</Form.Label>
                    <Form.Control 
                        type="text"
                        placeholder="Enter Name"
                        value={name}
                        onChange={(e) => setEmail(e.target.value)} >
                    </Form.Control>
                </Form.Group>
                <Form.Group controlId="email" className="my-3">
                    <Form.Label>Email Address</Form.Label>
                    <Form.Control 
                        type="email"
                        placeholder="Enter email"
                        value={email}
                        onChange={(e) => setEmail(e.target.value)} >
                    </Form.Control>
                </Form.Group>
                <Form.Group controlId="password" className="my-3">
                    <Form.Label>Password</Form.Label>
                    <Form.Control 
                        type="password"
                        placeholder="Enter password"
                        value={password}
                        onChange={(e) => setPassword(e.target.value)} >
                    </Form.Control>
                </Form.Group>
                <Form.Group controlId="confirmPassword" className="my-3">
                    <Form.Label>Confirm Password</Form.Label>
                    <Form.Control 
                        type="password"
                        placeholder="Confirm Password"
                        value={confirmPassword}
                        onChange={(e) => setConfirmPassword(e.target.value)} >
                    </Form.Control>
                </Form.Group>
                <Button type="submit" variant="primary" className="mt-2">
                    Register
                </Button>
                {isLoading && <Loader />}
            </Form>
            <Row className="py-3">
                <Col>
                    Already have an account ? {''}
                    <Link to = {redirect?`/login?redirect=${redirect}`:'/login'}> Login </Link>
                </Col>
            </Row>
        </FormContainer>
    )
}

export default RegisterScreen;
```

## Checkout process 
### Shipping Screen
update in **cartSlice.js** <br/>
**cartSlice.js** 
```
....
....
const initialState = localStorage.getItem('cart') ? JSON.parse(localStorage.getItem('cart')) : {cartItems: [], shippingAddress:{}, paymentMethod: 'PayPal'};
....
....
saveShippingAddress: (state, action) => {
      state.shippingAddress = action.payload;
      return updateCart(state)
    }
....
export const {addToCart, removeFromCart, saveShippingAddress} = cartSlice.actions;
```

create new file **ShippingScreen.js** under frontend/screens<br />
**ShippingScreen.js**
```
import {useState} from 'react';
import {Form, Button} from 'react-bootstrap';
import { useDispatch, useSelector } from 'react-redux';
import FormContainer from '../components/FormContainer';
import { useNavigate } from 'react-router-dom';
import { saveShippingAddress } from '../slices/cartSlice';

const ShippingAddress = () => {
    const cart = useSelector((state) => state.cart);
    const {shippingAddress} = cart;

    const [address, setAddress] = useState(shippingAddress?.address || '');
    const [city, setCity] = useState(shippingAddress?.city || '');
    const [postalCode, setPostalCode] = useState(shippingAddress?.postalCode || '');
    const [country, setCountry] = useState(shippingAddress?.country || '');

    const navigate = useNavigate();
    const dispatch = useDispatch();

    const submitHandler = (e) => {
        e.preventDefault();
        dispatch(saveShippingAddress ({
            address, city, postalCode, country    
        }));
        navigate('/payment');
    };

    return (
        <FormContainer>
            <h1>Shipping Address</h1>
            <Form onSubmit = {submitHandler}>
                <Form.Group controlId='address' className='my-2'>
                    <Form.Label>Address</Form.Label>
                    <Form.Control 
                        type = 'text'
                        placeholder='Enter Address'
                        value={address}
                        onChange={(e) => setAddress(e.target.value)}
                    ></Form.Control>
                </Form.Group>
                <Form.Group controlId='city' className='my-2'>
                    <Form.Label>City</Form.Label>
                    <Form.Control 
                        type = 'text'
                        placeholder='Enter City'
                        value={city}
                        onChange={(e) => setCity(e.target.value)}
                    ></Form.Control>
                </Form.Group>
                <Form.Group controlId='postalCode' className='my-2'>
                    <Form.Label>Postal Code</Form.Label>
                    <Form.Control 
                        type = 'text'
                        placeholder='Enter Postal Code'
                        value={postalCode}
                        onChange={(e) => setPostalCode(e.target.value)}
                    ></Form.Control>
                </Form.Group>
                <Form.Group controlId='country' className='my-2'>
                    <Form.Label>Country</Form.Label>
                    <Form.Control 
                        type = 'text'
                        placeholder='Enter Country'
                        value={country}
                        onChange={(e) => setCountry(e.target.value)}
                    ></Form.Control>
                </Form.Group>
                <Button type="submit" variant='primary' className='my-2'>Continue</Button>
            </Form>
        </FormContainer>
    )
}

export default ShippingAddress;
```

update in **index.js** <br/>
**index.js** <br/>
```
.....
import ShippingScreen from './screens/ShippingScreen.js';
.....
<Route path='/shipping' element={<ShippingScreen />} />
....
```

### Private Routes
create new file "**PrivateRoute.js**" under frontend/components <br/>
**PrivateRoute.js** 
```
import React from 'react'
import { Outlet, Navigate } from 'react-router-dom';
import { useSelector } from 'react-redux';

const PrivateRoute = () => {
    const {userInfo} = useSelector((state) => state.auth);
    return userInfo?<Outlet />:<Navigate to ='/login' replace />;
};

export default PrivateRoute;
```

and update in **index.js**
```
....
import PrivateRoute from './components/PrivateRoute.js';
....
<Route path='' element={<PrivateRoute />}>
        <Route path='/shipping' element={<ShippingScreen />} />
</Route>
....
```

### Checkout steps components
create new file "**CheckoutSteps.js**" under frontend/components <br />
**CheckoutSteps.js** <br />
```
import {Nav} from 'react-bootstrap';
import { LinkContainer } from 'react-router-bootstrap';

const CheckoutSteps = ({step1, step2, step3, step4}) => {
    return (
        <Nav className='justify-content-center mb-4'>
            <Nav.Item>
                {step1 ? (
                    <LinkContainer to = '/login'>
                        <Nav.Link>Sign In</Nav.Link>
                    </LinkContainer>
                ) : (
                    <Nav.Link disabled>Sign In</Nav.Link>
                )}
            </Nav.Item>
            <Nav.Item>
                {step2 ? (
                    <LinkContainer to='/shipping'>
                        <Nav.Link>Shipping</Nav.Link>
                    </LinkContainer>
                ) : (
                    <Nav.Link disabled>Shipping</Nav.Link>
                )}
            </Nav.Item>
            <Nav.Item>
                {step3 ? (
                    <LinkContainer to = '/payment'>
                        <Nav.Link>payment</Nav.Link>
                    </LinkContainer>
                ) : (
                    <Nav.Link disabled>payment</Nav.Link>
                )}
            </Nav.Item>
            <Nav.Item>
                {step4 ? (
                    <LinkContainer to = '/placeholder'>
                        <Nav.Link>Place Order</Nav.Link>
                    </LinkContainer>
                ) : (
                    <Nav.Link disabled>Place Order</Nav.Link>
                )}
            </Nav.Item>
        </Nav>
    )
}
export default CheckoutSteps
```

**ShippingScreen.js** <br />
```
.....
import CheckoutSteps from '../components/CheckoutSteps';
.....
.....
return (
        <FormContainer>
            <CheckoutSteps step1 step2 />
            <h1>Shipping Address</h1>
.....
```

### Payment Method
udpate in **cartSlice.js** <br />
```
.....
return updateCart(state);
},
savePaymentMethod: (state, action) => {
      state.paymentMethod = action.payload;
      return updateCart(state);
    },
.....
.....
export const {addToCart, removeFromCart, saveShippingAddress, savePaymentMethod} = cartSlice.actions;
```

create new file **PaymentScreen.js** under frontend/screens <br />
**PaymentScreen.js**
```
import {useState, useEffect} from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { useNavigate } from 'react-router-dom';
import {Form, Button, Col} from 'react-bootstrap';
import FormContainer from '../components/FormContainer';
import CheckoutSteps from '../components/CheckoutSteps';
import { savePaymentMethod } from '../slices/cartSlice';

const PaymentScreen = () => {
    const [paymentMethod, setPaymentMethod] = useState('PayPal');
    const dispatch = useDispatch();
    const navigate = useNavigate();

    const cart = useSelector((state) => state.cart);
    const {shippingAddress} = cart;

    useEffect(() => {
        if(!shippingAddress) {
            navigate('/shipping');
        }
    }, [shippingAddress, navigate]);

    const submitHandler = (e) => {
        e.preventDefault();
        dispatch(savePaymentMethod(paymentMethod));
        navigate('/placeholder');
    };

    return (
        <FormContainer>
            <CheckoutSteps step1 step2 step3 step4 />
            <h1>Payment Method</h1>
            <Form onSubmit = {submitHandler}>
                <Form.Group>
                    <Form.Label as='Legend'>Select Method</Form.Label>
                    <Col>
                        <Form.Check 
                            type='radio'
                            className='my-2'
                            label='PayPal or Credit Card'
                            id='PayPal'
                            name='paymentMethod'
                            value='PayPal'
                            checked
                            onChange = {(e) => setPaymentMethod(e.target.value)}>
                        </Form.Check>
                    </Col>
                </Form.Group>
                <Button type='submit' variant='primary'>Continue</Button>
            </Form>
        </FormContainer>
    )
}

export default PaymentScreen;
```

update in **index.js** <br />
**index.js** 
```
.....
import PaymentScreen from './screens/PaymentScreen.js';
......
......
<Route path='/payment' element={<PaymentScreen />} />
......
```

### Order Routes & Controller
create new file **orderController.js** under backend/controllers <br />
**oderController.js** 
```
import asyncHandler from '../middleware/asyncHandler.js'
import Order from '../models/orderModel.js';

//@desc create new order
//@route POST /api/orders
//@access Private
const addOrderItems = asyncHandler(async(req, res) => {
    res.send('add order items');
});

//@desc Get logged in user orders
//@route GET /api/orders/myorders
//@access Private
const getMyOrders = asyncHandler(async(req, res) => {
    res.send('get my orders');
});

//@desc Get order by ID
//@route GET /api/orders/:id
//@access Private
const getOrderById = asyncHandler(async(req, res) => {
    res.send('get order by id');
});

//@desc Update order to paid
//@route GET /api/orders/:id/pay
//@access Private
const updateOrderToPaid = asyncHandler(async(req, res) => {
    res.send('update order to paid');
});

//@desc update order to delivered
//@route GET /api/orders/:id/deliver
//@access Private/Admin
const updateOrderToDelivered = asyncHandler(async(req, res) => {
    res.send('update order to delivered');
});

//@desc Get all orders
//@route GET /api/orders
//@access Private/Admin
const getOrders = asyncHandler(async(req, res) => {
    res.send('get all orders');
});

export {
    addOrderItems,
    getMyOrders,
    getOrderById,
    updateOrderToPaid,
    updateOrderToDelivered,
    getOrders
};
```

create new file **orderRoutes.js** under backend/routes <br />
**oderRoutes.js** 
```
import express from 'express';
const router = express.Router();
import {
    addOrderItems,
    getMyOrders,
    getOrderById,
    updateOrderToPaid,
    updateOrderToDelivered,
    getOrders
} from '../controllers/orderController.js'
import {protect, admin} from '../middleware/authMiddleware.js';

router.route('/').post(protect, addOrderItems).get(protect, admin, getOrders);
router.route('/mine').get(protect, getMyOrders);
router.route('/:id').get(protect, admin, getOrderById);
router.route('/:id/pay').put(protect, updateOrderToPaid);
router.route('/:id/deliver').put(protect, admin, updateOrderToDelivered);

export default router;
```

update in **server.js** <br />
**server.js** <br />
```
.....
import orderRoutes from './routes/orderRoutes.js';
.....
app.use('/api/orders', orderRoutes);
.....
```

### Create & Get Orders
update in **orderController.js** <br />
**orderController.js**
```.
import asyncHandler from '../middleware/asyncHandler.js'
import Order from '../models/orderModel.js';

//@desc create new order
//@route POST /api/orders
//@access Private
const addOrderItems = asyncHandler(async(req, res) => {
    const {
        orderItems,
        shippingAddress,
        paymentMethod,
        itemsPrice,
        taxPrice,
        shippingPrice,
        totalPrice
    } = req.body;

    if(orderItems && orderItems.length === 0) {
        res.status(400);
        throw new Error('No order items');
    } else {
        const order = new Order ({
            orderItems : orderItems.map((x) => ({
                ...x,
                product: x._id,
                _id: undefined
            })),
            user: req.user._id,
            shippingAddress,
            paymentMethod,
            itemsPrice,
            taxPrice,
            shippingPrice,
            totalPrice
        });
        const createdOrder = await order.save();
        res.status(201).json(createdOrder);
    }   
});

//@desc Get logged in user orders
//@route GET /api/orders/myorders
//@access Private
const getMyOrders = asyncHandler(async(req, res) => {
    const orders = await Order.find({user: req.user._id});
    res.status(200).json(orders);
});

//@desc Get order by ID
//@route GET /api/orders/:id
//@access Private
const getOrderById = asyncHandler(async(req, res) => {
    const order = await Order.findById(req.params.id).populate(
        'user',
        'name email'
    );
    if(order) {
        res.status(200).json(order);
    } else {
        res.status(404);
        throw new Error('Order not found');
    }
});

//@desc Update order to paid
//@route GET /api/orders/:id/pay
//@access Private
const updateOrderToPaid = asyncHandler(async(req, res) => {
    res.send('update order to paid');
});

//@desc update order to delivered
//@route GET /api/orders/:id/deliver
//@access Private/Admin
const updateOrderToDelivered = asyncHandler(async(req, res) => {
    res.send('update order to delivered');
});

//@desc Get all orders
//@route GET /api/orders
//@access Private/Admin
const getOrders = asyncHandler(async(req, res) => {
    res.send('get all orders');
});

export {
    addOrderItems,
    getMyOrders,
    getOrderById,
    updateOrderToPaid,
    updateOrderToDelivered,
    getOrders
};
```
