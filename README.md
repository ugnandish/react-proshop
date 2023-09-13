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
    comment: {
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
                        onChange={(e) => setName(e.target.value)} >
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
        navigate('/placeorder');
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

### Order API Slice & Start Order Screen
create new file "**ordersApiSlice.js**" under frontend/slices <br />
**ordersApiSlice.js** <br />
```
import {apiSlice} from './apiSlice';
import { ORDERS_URL } from '../constants';

export const ordersApiSlice = apiSlice.injectEndpoints ({
    endpoints: (builder) => ({
        createOrder: builder.mutation ({
            query: (order) => ({
                url: ORDERS_URL,
                method: 'POST',
                body: {...order}
            })
        })
    })
});

export const {useCreateOrderMutation} = ordersApiSlice;
```

update in **cartSlice.js** <br />
**cartSlice.js** <br />
```
....
clearCartItems: (state, action) => {
      state.cartItems = [];
      return updateCart(state);
    }
.....
export const {addToCart, removeFromCart, saveShippingAddress, savePaymentMethod, clearCartItems} = cartSlice.actions;
```

create new file **PlaceOrderScreen.js** under frontend/screens <br />
**PlaceOrderScreen.js** <br />
```
import React from 'react';
import {useState, useEffect} from 'react';
import {Link, useNavigate} from 'react-router-dom';
import { UseSelector, useSelector } from 'react-redux';
import { Button, Row, Col, ListGroup, Image, Card } from 'react-bootstrap';
import CheckoutSteps from '../components/CheckoutSteps';

const PlaceOrderScreen = () => {
    const navigate = useNavigate();
    const cart = useSelector((state) => state.cart);

    useEffect(() => {
        if(!cart.shippingAddress.address) {
            navigate('/shipping');
        } else if(!cart.paymentMethod) {
            navigate('/payment');
        }
    }, [cart.paymentMethod, cart.shippingAddress.address, navigate]);

    return (
        <>
            <CheckoutSteps step1 step2 step3 step4 />
            <Row>
                <Col md={8}>Column</Col>
                <Col md={4}>Column</Col>
            </Row>
        </>
    )
}

export default PlaceOrderScreen;
```

update in **index.js** <br />
**index.js** <br />
```
....
import PlaceOrderScreen from './screens/PlaceOrderScreen.js';
....
<Route path='/placeorder' element={<PlaceOrderScreen />} />
....
```

### Creating an Order
modify the **PlaceOrderScreen.js** <br />
**PlaceOrderScreen.js** 
```
import React from 'react';
import {useState, useEffect} from 'react';
import {Link, useNavigate} from 'react-router-dom';
import { useSelector, useDispatch } from 'react-redux';
import { Button, Row, Col, ListGroup, Image, Card } from 'react-bootstrap';
import {toast} from 'react-toastify';
import CheckoutSteps from '../components/CheckoutSteps';
import Message from '../components/Message';
import Loader from '../components/Loader';
import {useCreateOrderMutation} from '../slices/ordersApiSlice';
import { clearCartItems } from '../slices/cartSlice';

const PlaceOrderScreen = () => {
    const navigate = useNavigate();
    const dispatch = useDispatch();
    const cart = useSelector((state) => state.cart);
    const [createOrder, {isLoading, error}] = useCreateOrderMutation();

    useEffect(() => {
        if(!cart.shippingAddress.address) {
            navigate('/shipping');
        } else if(!cart.paymentMethod) {
            navigate('/payment');
        }
    }, [cart.paymentMethod, cart.shippingAddress.address, navigate]);

    const placeOrderHandler = async() => {
        try {
            const res = await createOrder ({
                orderItems: cart.cartItems,
                shippingAddress: cart.shippingAddress,
                paymentMethod: cart.paymentMethod,
                itemsPrice: cart.itemsPrice,
                shippingPrice: cart.shippingPrice,
                taxPrice: cart.taxPrice,
                totalPrice: cart.totalPrice
            }).unwrap();
            dispatch(clearCartItems());
            navigate(`/order/${res._id}`);
        } catch (error) {
            toast.error(error);
        }
    };

    return (
        <>
            <CheckoutSteps step1 step2 step3 step4 />
            <Row>
                <Col md={8}>
                    <ListGroup variant='flush'>
                        <ListGroup.Item>
                            <h2>Shipping</h2>
                            <p>
                                <strong>Address: </strong>
                                {cart.shippingAddress.address}, {cart.shippingAddress.city}{' '}
                                {cart.shippingAddress.postalCode},{' '} {cart.shippingAddress.country}
                            </p>
                        </ListGroup.Item>
                        <ListGroup.Item>
                            <h2>Payment Method</h2>
                            <strong>Method: </strong>
                            {cart.paymentMethod}
                        </ListGroup.Item>
                        <ListGroup.Item>
                            <h2>Order Items</h2>
                            {cart.cartItems.length === 0 ? (
                                <message>Your cart is empty</message>
                            ) : (
                                <ListGroup variant='flush'>
                                    {cart.cartItems.map((item, index) => (
                                        <ListGroup.Item key={index}>
                                            <Row>
                                                <Col md={1}>
                                                    <Image src={item.image} alt={item.name} fluid rounded />
                                                </Col>
                                                <Col>
                                                    <Link to={`/products/${item.product}`}>
                                                        {item.name}
                                                    </Link>
                                                </Col>
                                                <Col md={4}>
                                                    {item.qty} * {item.price} = ${item.qty*item.price}
                                                </Col>
                                            </Row>
                                        </ListGroup.Item>
                                    ))}
                                </ListGroup>
                            )}
                        </ListGroup.Item>
                    </ListGroup>
                </Col>
                <Col md={4}>
                    <Card>
                        <ListGroup variant='flush'>
                            <ListGroup.Item>
                                <h2>Order Summary</h2>
                            </ListGroup.Item>
                            <ListGroup.Item>
                                <Row>
                                    <Col>Items:</Col>
                                    <Col>${cart.itemsPrice}</Col>
                                </Row>
                            </ListGroup.Item>
                            <ListGroup.Item>
                                <Row>
                                    <Col>Shipping:</Col>
                                    <Col>${cart.shippingPrice}</Col>
                                </Row>
                            </ListGroup.Item>
                            <ListGroup.Item>
                                <Row>
                                    <Col>Tax:</Col>
                                    <Col>${cart.taxPrice}</Col>
                                </Row>
                            </ListGroup.Item>
                            <ListGroup.Item>
                                <Row>
                                    <Col>Total:</Col>
                                    <Col>${cart.totalPrice}</Col>
                                </Row>
                            </ListGroup.Item>
                            <ListGroup.Item>
                                <Button type='button' className='btn-block' disabled={cart.cartItems.length === 0} onClick = {placeOrderHandler}>
                                    Place Order
                                </Button>
                            </ListGroup.Item>
                        </ListGroup>
                    </Card>
                </Col>
            </Row>
        </>
    )
}

export default PlaceOrderScreen;
```

## Checkout process - part 2
### Order Page
**orderRoutes.js** <br />
remove **admin** from below line <br />
```
.....
router.route('/:id').get(protect, getOrderById);
.....
```

update in **orderApiSlice.js** <br/>
**orderApiSlice.js**
```
....
....
getOrderDetails: builder.query ({
            query: (orderId) => ({
                url: `${ORDERS_URL}/${orderId}`
            }),
            keepUnusedDataFor: 5
        })
    })
});

export const {useCreateOrderMutation, useGetOrderDetailsQuery} = ordersApiSlice;
```

create new file **OrderScreen.js** under frontend/screens <br />
**OrderScreen.js** <br />
```
import React from 'react'
import {Link, useParams} from 'react-router-dom';
import {Row, Col, ListGroup, Image, Form, Button, Card} from 'react-bootstrap';
import Message from '../components/Message';
import Loader from '../components/Loader';
import { useGetOrderDetailsQuery } from '../slices/ordersApiSlice';

const OrderScreen = () => {
    const {id: orderId} = useParams();
    const {data:order, refetch, isLoading, isError} = useGetOrderDetailsQuery(orderId);
    
    return isLoading ? (
        <Loader />
    ) : Error ? (
        <Message variant='danger'>india</Message>
    ) : (
        <>
            <h1>Order {order._id}</h1>
            <Row>
                <Col md={8}>
                    <ListGroup variant='flush'>
                        <ListGroup.Item>
                            <h2>Shipping</h2>
                            <p><strong>Name:</strong> {order.user.name}</p>
                            <p><strong>Email:</strong> {order.user.email}</p>
                            <p><strong>Address:</strong> 
                                {order.shippingAddress.address}, {order.shippingAddress.city}{' '}
                                {order.shippingAddress.postalCode}, {' '}
                                {order.shippingAddress.country}
                            </p>
                            {order.isDelivered ? (
                                <Message variant='success'>
                                    Delivered on {order.deliveredAt}
                                </Message>
                            ) : (
                                <Message variant='danger'>Not delivered</Message>
                            )}
                        </ListGroup.Item>
                        <ListGroup.Item>
                            <h2>Payment Method</h2>
                            <p><strong>Method:</strong>
                                {order.paymentMethod}
                            </p>
                            {order.isPaid ? (
                                <Message variant='success'>Paid on {order.paidAt}</Message>
                            ) : (
                                <Message variant='danger'>Not Paid</Message>
                            )}
                        </ListGroup.Item>
                        <ListGroup.Item>
                            <h2>Order Items</h2>
                            {order.orderItems.map((item, index) => {
                                <ListGroup.Item key={index}>
                                    <Row>
                                        <Col md={1}>
                                            <Image src={item.image} alt={item.name} fluid rounded />
                                        </Col>
                                        <Col>
                                            <Link to={`/product/${item.product}`}>
                                                {item.name}
                                            </Link>
                                        </Col>
                                        <Col md={4}>
                                            {item.qty} * {item.price} = ${item.qty * item.price}
                                        </Col>
                                    </Row>
                                </ListGroup.Item>
                            })}
                        </ListGroup.Item>
                    </ListGroup>
                </Col>
                <Col md={4}>
                    <Card>
                        <ListGroup variant='flush'>
                            <ListGroup.Item>
                                <h2>Order Summary</h2>
                            </ListGroup.Item>
                            <ListGroup.Item>
                                <Row>
                                    <Col>Items</Col>
                                    <Col>${order.itemsPrice}</Col>
                                </Row>
                                <Row>
                                    <Col>Shipping</Col>
                                    <Col>${order.shippingPrice}</Col>
                                </Row>
                                <Row>
                                    <Col>Tax</Col>
                                    <Col>${order.taxPrice}</Col>
                                </Row>
                                <Row>
                                    <Col>Total</Col>
                                    <Col>{order.totalPrice}</Col>
                                </Row>
                            </ListGroup.Item>
                        </ListGroup>
                    </Card>
                </Col>
            </Row>
        </>
    )
}

export default OrderScreen;
```

update in **index.js** <br />
**index.js**
```
....
....
import OrderScreen from './screens/OrderScreen.js';
.....
<Route path='/order/:id' element={<OrderScreen />} />
.....
```

### PayPal Setup & Order Paid
update in **orderController.js** <br />
```
....
....
//@desc Update order to paid
//@route PUT /api/orders/:id/pay
//@access Private
const updateOrderToPaid = asyncHandler(async(req, res) => {
    const order = await Order.findById(req.params.id);
    if(order) {
        order.isPaid = true;
        order.paidAt = Date.now();
        order.paymentResult = {
            id: req.body.id,
            status: req.body.status,
            update_time: req.body.update_time,
            email_address: req.body.payer.email_address,
        };
        const updateOrder = await order.save();
        res.status(200).json(updatedOrder);
    } else {
        res.status(404);
        throw new Error('Order not found');
    }
});
....
```

update in **.env** <br />
.env
```
PAYPAL_CLIENT_ID = (copy from paypal account)
```

**server.js** <br />
```
....
app.get('/api/config/paypal', (req, res) => {
    res.send({clientId:process.env.PAYPAL_CLIENT_ID})
});
.....
```

### React-PayPal Integration
install paypal under frontend <br />
**npm i @paypal/react-paypal-js** <br />

**index.js** <br/>
```
....
import {PayPalScriptProvider} from '@paypal/react-paypal-js';
....
....
<Provider store={store} >
      <PayPalScriptProvider deferLoading={true}>
        <RouterProvider router={router} />
      </PayPalScriptProvider>
    </Provider>
....
```

update in **OrderScreen.js** <br />
**OrderScreen.js**
```
import React, {useState, useEffect} from 'react'
import {Row, Col, ListGroup, Image, Form, Button, Card} from 'react-bootstrap';
import {toast} from 'react-toastify';
import { useSelector } from 'react-redux';
import { PayPalButtons, usePayPalScriptReducer } from '@paypal/react-paypal-js';
import {Link, useParams} from 'react-router-dom';
import Message from '../components/Message';
import Loader from '../components/Loader';
import { useGetOrderDetailsQuery, usePayOrderMutation, useGetPaypalClientIdQuery } from '../slices/ordersApiSlice';

const OrderScreen = () => {
    const {id: orderId} = useParams();
    const {data: order, refetch, isLoading, error} = useGetOrderDetailsQuery(orderId);
    const [payOrder, {isLoading:loadingPay}] = usePayOrderMutation();
    const [{isPending}, paypalDispatch] = usePayPalScriptReducer();
    const {
        data: paypal,
        isLoading: loadingPayPal,
        error: errorPayPal,
    } = useGetPaypalClientIdQuery();
    const {userInfo} = useSelector((state) => state.auth);
    useEffect(() => {
        if(!error.PayPal && !loadingPayPal && paypal.clientId) {
            const loadingPayPalScript = async () => {
                paypalDispatch ({
                    type: 'resetOptions',
                    value: {
                        'client-id': paypal.clientId,
                        currency: 'USD',
                    }
                });
                paypalDispatch({type: 'setLoadingStatus', value: 'pending'});
            }
            if(order && !order.isPaid) {
                if(!window.paypal) {
                    loadingPayPalScript();
                }
            }
        }
    }, [order, paypal, paypalDispatch, loadingPayPal, errorPayPal]);

.....
....
```

### PayPal Button
update in **orderApiSlice.js** <br/>
**orderApiSlice.js** <br />
```
payOrder: builder.mutation({
            query: ({orderId, details}) => ({
```

update in **OrderScreen.js** <br />
**OrderScreen.js**
```
import React, {useState, useEffect} from 'react'
import {Row, Col, ListGroup, Image, Form, Button, Card} from 'react-bootstrap';
import {toast} from 'react-toastify';
import { useSelector } from 'react-redux';
import { PayPalButtons, usePayPalScriptReducer } from '@paypal/react-paypal-js';
import {Link, useParams} from 'react-router-dom';
import Message from '../components/Message';
import Loader from '../components/Loader';
import { useGetOrderDetailsQuery, usePayOrderMutation, useGetPayPalClientIdQuery } from '../slices/ordersApiSlice';

const OrderScreen = () => {
    const {id: orderId} = useParams();
    const {data: order, refetch, isLoading, error} = useGetOrderDetailsQuery(orderId);
    const [payOrder, {isLoading:loadingPay}] = usePayOrderMutation();
    const [{isPending}, paypalDispatch] = usePayPalScriptReducer();
    const {
        data: paypal,
        isLoading: loadingPayPal,
        error: errorPayPal,
    } = useGetPayPalClientIdQuery();

    const {userInfo} = useSelector((state) => state.auth);
    
    useEffect(() => {
        if(!errorPayPal && !loadingPayPal && paypal.clientId) {
            const loadingPayPalScript = async () => {
                paypalDispatch ({
                    type: 'resetOptions',
                    value: {
                        'client-id': paypal.clientId,
                        currency: 'USD',
                    }
                });
                paypalDispatch({type: 'setLoadingStatus', value: 'pending'});
            }
            if(order && !order.isPaid) {
                if(!window.paypal) {
                    loadingPayPalScript();
                }
            }
        }
    }, [order, paypal, paypalDispatch, loadingPayPal, errorPayPal]);

    function onApprove(data, actions) {
        return actions.order.capture().then(async function(details) {
            try {
                await payOrder({orderId, details});
                refetch();
                toast.success('Payment Successful');
            } 
            catch (err) {
                toast.error(err?.data?.message || err.message);
            }
        })
    }

    async function onApproveTest() {
        await payOrder({orderId, details:{payer:{}}});
        refetch();
        toast.success('Payment Successful');
    }

    function onError(err) {
        toast.error(err.message);
    }

    function createOrder(data, actions) {
        return actions.order.create ({
            purchase_units: [
                {
                    amount: {
                        value: order.totalPrice,
                    }
                }
            ]
        }).then((orderId) => {
            return orderId;
        });
    }
    
    return isLoading ? (
        <Loader />
    ) : error ? (
        <Message variant='danger' />
    ) : (
        <>
            <h1>Order {order._id}</h1>
            <Row>
                <Col md={8}>
                    <ListGroup variant='flush'>
                        <ListGroup.Item>
                            <h2>Shipping</h2>
                            <p><strong>Name:</strong> {order.user.name}</p>
                            <p><strong>Email:</strong> {order.user.email}</p>
                            <p><strong>Address:</strong> 
                                {order.shippingAddress.address}, {order.shippingAddress.city}{' '}
                                {order.shippingAddress.postalCode}, {' '}
                                {order.shippingAddress.country}
                            </p>
                            {order.isDelivered ? (
                                <Message variant='success'>
                                    Delivered on {order.deliveredAt}
                                </Message>
                            ) : (
                                <Message variant='danger'>Not delivered</Message>
                            )}
                        </ListGroup.Item>
                        <ListGroup.Item>
                            <h2>Payment Method</h2>
                            <p><strong>Method:</strong>
                                {order.paymentMethod}
                            </p>
                            {order.isPaid ? (
                                <Message variant='success'>Paid on {order.paidAt}</Message>
                            ) : (
                                <Message variant='danger'>Not Paid</Message>
                            )}
                        </ListGroup.Item>
                        <ListGroup.Item>
                            <h2>Order Items</h2>
                            {order.orderItems.map((item, index) => {
                                <ListGroup.Item key={index}>
                                    <Row>
                                        <Col md={1}>
                                            <Image src={item.image} alt={item.name} fluid rounded />
                                        </Col>
                                        <Col>
                                            <Link to={`/product/${item.product}`}>
                                                {item.name}
                                            </Link>
                                        </Col>
                                        <Col md={4}>
                                            {item.qty} * {item.price} = ${item.qty * item.price}
                                        </Col>
                                    </Row>
                                </ListGroup.Item>
                            })}
                        </ListGroup.Item>
                    </ListGroup>
                </Col>
                <Col md={4}>
                    <Card>
                        <ListGroup variant='flush'>
                            <ListGroup.Item>
                                <h2>Order Summary</h2>
                            </ListGroup.Item>
                            <ListGroup.Item>
                                <Row>
                                    <Col>Items</Col>
                                    <Col>${order.itemsPrice}</Col>
                                </Row>
                                <Row>
                                    <Col>Shipping</Col>
                                    <Col>${order.shippingPrice}</Col>
                                </Row>
                                <Row>
                                    <Col>Tax</Col>
                                    <Col>${order.taxPrice}</Col>
                                </Row>
                                <Row>
                                    <Col>Total</Col>
                                    <Col>{order.totalPrice}</Col>
                                </Row>
                            </ListGroup.Item>
                            {!order.isPaid && (
                                <ListGroup.Item>
                                    {loadingPay && <Loader />}
                                    {isPending ? <Loader /> : (
                                        <div>
                                            <Button onClick={onApproveTest} style={{marginBottom:"10px"}}>
                                                Test Pay Order
                                            </Button>
                                            <div>
                                                <PayPalButtons 
                                                    createOrder={createOrder}
                                                    onApprove={onApprove}
                                                    onError={onError}
                                                ></PayPalButtons>
                                            </div>
                                        </div>
                                    )}
                                </ListGroup.Item>
                            )}
                        </ListGroup>
                    </Card>
                </Col>
            </Row>
        </>
    )
}

export default OrderScreen;
```

### User Profile & Update
update in **usersApiSlice.js** <br />
**usersApiSlice.js**
```
....
....
profile: builder.mutation ({
            query: (data) => ({
                url: `${USERS_URL}/profile`,
                method: 'PUT',
                body: data,
            })
        })
    })
})

export const {useLoginMutation, useLogoutMutation, useRegisterMutation, useProfileMutation} = usersApiSlice;
```

create new file **ProfileScreen.js** under frontend/screens <br />
**ProfileScreen.js** <br />
```
import React, {useState, useEffect} from 'react'
import {Table, Form, Button, Row, Col} from 'react-bootstrap';
import { LinkContainer } from 'react-router-bootstrap';
import { useDispatch, useSelector } from 'react-redux';
import {toast} from 'react-toastify';
import Message from '../components/Message'; 
import Loader from '../components/Loader';
import { useProfileMutation } from '../slices/usersApiSlice';
import { setCredentials } from '../slices/authSlice';

const ProfileScreen = () => {
    const [name, setName] = useState("");
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");
    const [confirmPassword, setConfirmPassword] = useState("");

    const dispatch = useDispatch();

    const {userInfo} = useSelector((state) => state.auth);

    const [updateProfile, {isLoading:loadingUpdateProfile}] = useProfileMutation();

    useEffect(() => {
        if(userInfo) {
            setName(userInfo.name);
            setEmail(userInfo.email);
        }
    }, [userInfo, userInfo.name, userInfo.email]);
    
    const submitHandler = async (e) => {
        e.preventDefault();
        if(password !== confirmPassword) {
            toast.error('Password do not match');
        } else {
            try {
                const res = await updateProfile({_id:userInfo._id, name, email, password}).unwrap();
                dispatch(setCredentials(res));
                toast.success('Profile updated successfully');
            } catch (error) {
                toast.error(error?.data?.message || error.error);
            }
        }
    }

    return (
        <Row>
            <Col md={3}>
                <h2>User Profile</h2>
                <Form onSubmit={submitHandler}>
                    <Form.Group controlId='name' className='my-2'>
                        <Form.Label>Name</Form.Label>
                        <Form.Control
                            type='name' 
                            placeholder='Enter Name' 
                            value={name} 
                            onChange={(e) => setName(e.target.value)}>
                        </Form.Control>
                    </Form.Group>
                    <Form.Group controlId='email' className='my-2'>
                        <Form.Label>Email Address</Form.Label>
                        <Form.Control
                            type='email' 
                            placeholder='Enter Email' 
                            value={email} 
                            onChange={(e) => setEmail(e.target.value)}>
                        </Form.Control>
                    </Form.Group>
                    <Form.Group controlId='password' className='my-2'>
                        <Form.Label>Password</Form.Label>
                        <Form.Control
                            type='password' 
                            placeholder='Enter Password' 
                            value={password} 
                            onChange={(e) => setPassword(e.target.value)}>
                        </Form.Control>
                    </Form.Group>
                    <Form.Group controlId='confirmPassword' className='my-2'>
                        <Form.Label>Confirm Password</Form.Label>
                        <Form.Control
                            type='password' 
                            placeholder='Confirm Password' 
                            value={confirmPassword} 
                            onChange={(e) => setConfirmPassword(e.target.value)}>
                        </Form.Control>
                    </Form.Group>
                    <Button type='submit' variant='primary' className='my-2'>
                        Update
                    </Button>
                    {loadingUpdateProfile && <Loader />}
                </Form>
            </Col>
        </Row>
    )
}

export default ProfileScreen
```

### Display Order History
update in **ordersApiSlice.js** <br />
**ordersApiSlice.js** <br/>
```
.....
.....
        }),
        getMyOrders: builder.query ({
            query: () => ({
                url: `${ORDERS_URL}/mine`,
            }),
            keepUnusedDataFor: 5,
        })
    }),
});

export const {
    useCreateOrderMutation, 
    useGetOrderDetailsQuery, 
    usePayOrderMutation, 
    useGetPayPalClientIdQuery,
    useGetMyOrdersQuery
} = ordersApiSlice;
```

update in **ProfileScreen.js** <br />
**ProfileScreen.js** <br />
```
import React, {useState, useEffect} from 'react'
import {Table, Form, Button, Row, Col} from 'react-bootstrap';
import { LinkContainer } from 'react-router-bootstrap';
import { useDispatch, useSelector } from 'react-redux';
import {toast} from 'react-toastify';
import Message from '../components/Message'; 
import Loader from '../components/Loader';
import { FaTimes } from 'react-icons/fa';
import { useProfileMutation } from '../slices/usersApiSlice';
import { setCredentials } from '../slices/authSlice';
import { useGetMyOrdersQuery } from '../slices/ordersApiSlice';

const ProfileScreen = () => {
    const [name, setName] = useState("");
    const [email, setEmail] = useState("");
    const [password, setPassword] = useState("");
    const [confirmPassword, setConfirmPassword] = useState("");

    const dispatch = useDispatch();

    const {userInfo} = useSelector((state) => state.auth);

    const [updateProfile, {isLoading:loadingUpdateProfile}] = useProfileMutation();
    const {data:orders, isLoading, error} = useGetMyOrdersQuery();

    useEffect(() => {
        if(userInfo) {
            setName(userInfo.name);
            setEmail(userInfo.email);
        }
    }, [userInfo, userInfo.name, userInfo.email]);
    
    const submitHandler = async (e) => {
        e.preventDefault();
        if(password !== confirmPassword) {
            toast.error('Password do not match');
        } else {
            try {
                const res = await updateProfile({_id:userInfo._id, name, email, password}).unwrap();
                dispatch(setCredentials(res));
                toast.success('Profile updated successfully');
            } catch (error) {
                toast.error(error?.data?.message || error.error);
            }
        }
    }

    return (
        <Row>
            <Col md={3}>
                <h2>User Profile</h2>
                <Form onSubmit={submitHandler}>
                    <Form.Group controlId='name' className='my-2'>
                        <Form.Label>Name</Form.Label>
                        <Form.Control
                            type='name' 
                            placeholder='Enter Name' 
                            value={name} 
                            onChange={(e) => setName(e.target.value)}>
                        </Form.Control>
                    </Form.Group>
                    <Form.Group controlId='email' className='my-2'>
                        <Form.Label>Email Address</Form.Label>
                        <Form.Control
                            type='email' 
                            placeholder='Enter Email' 
                            value={email} 
                            onChange={(e) => setEmail(e.target.value)}>
                        </Form.Control>
                    </Form.Group>
                    <Form.Group controlId='password' className='my-2'>
                        <Form.Label>Password</Form.Label>
                        <Form.Control
                            type='password' 
                            placeholder='Enter Password' 
                            value={password} 
                            onChange={(e) => setPassword(e.target.value)}>
                        </Form.Control>
                    </Form.Group>
                    <Form.Group controlId='confirmPassword' className='my-2'>
                        <Form.Label>Confirm Password</Form.Label>
                        <Form.Control
                            type='password' 
                            placeholder='Confirm Password' 
                            value={confirmPassword} 
                            onChange={(e) => setConfirmPassword(e.target.value)}>
                        </Form.Control>
                    </Form.Group>
                    <Button type='submit' variant='primary' className='my-2'>
                        Update
                    </Button>
                    {loadingUpdateProfile && <Loader />}
                </Form>
            </Col>
            <Col md={9}>
                {isLoading ? (
                    <Loader />
                ) : error ? (
                    <Message variant='danger'>
                        {error?.data?.message || error.error}
                    </Message> ) : (
                        <Table stripped hover responsive className='table-sm'>
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>DATE</th>
                                    <th>TOTAL</th>
                                    <th>PAID</th>
                                    <th>DELIVERED</th>
                                    <th></th>
                                </tr>
                            </thead>
                            <tbody>
                                {orders.map((order) => (
                                    <tr key={order._id}>
                                        <td>{order._id}</td>
                                        <td>{order.createdAt.substring(0,10)}</td>
                                        <td>${order.totalPrice}</td>
                                        <td>
                                            {order.isPaid ? (
                                                order.paidAt.substring(0,10)
                                            ) :(
                                                <FaTimes style={{color: 'red'}} />
                                            )}
                                        </td>
                                        <td>
                                            {order.isDelivered ? (
                                                order.deliveredAt.substring(0,10)
                                            ) : (
                                                <FaTimes style={{color: 'red'}} />
                                            )}
                                        </td>
                                        <td>
                                            <LinkContainer to={`/order/${order._id}`}>
                                                <Button className='btn-sm' variant='light'>
                                                    Details
                                                </Button>
                                            </LinkContainer>
                                        </td>
                                    </tr>
                                ))}
                            </tbody>
                        </Table>
                    )
                }
            </Col>
        </Row>
    )
}

export default ProfileScreen
```

## Admin Functionality
### Admin Route Functionality
create new file **AdminRoute.js** under frontend/components <br />
**AdminRoute.js** <br/>
```
import React from 'react'
import { Outlet, Navigate } from 'react-router-dom';
import { useSelector } from 'react-redux';

const AdminRoute = () => {
    const {userInfo} = useSelector((state) => state.auth);
    return userInfo && userInfo.isAdmin ? (
        <Outlet /> 
    ) : ( 
        <Navigate to ='/login' replace /> 
    );
};

export default AdminRoute;
```

create new folder **admin** under frontend/screens <br />
create new file **OrderListScreen.js** under frontend/screens/admin <br />
**OrderListScreen.js** <br />
```
import React from 'react'
const OrderListScreen = () => {
  return (
    <div>OrderListScreen</div>
  )
}
export default OrderListScreen
```

update in **index.js** <br/>
```
....
import PrivateRoute from './components/PrivateRoute.js';
import AdminRoute from './components/AdminRoute.js';
....
import OrderListScreen from './screens/admin/OrderListScreen.js';
....
....
<Route path='' element={<AdminRoute />}>
        <Route path='/admin/orderlist' element={<OrderListScreen />} />
      </Route>
```

update in **Header.js** <br />
**Header.js** <br />
```
 </LinkContainer>
              )}
              {userInfo && userInfo.isAdmin && (
                <NavDropdown title='Admin' id='adminmenu'>
                  <LinkContainer to='/admin/productlist'>
                    <NavDropdown.Item>Products</NavDropdown.Item>
                  </LinkContainer>
                  <LinkContainer to='/admin/userlist'>
                    <NavDropdown.Item>Users</NavDropdown.Item>
                  </LinkContainer>
                  <LinkContainer to='/admin/orderlist'>
                    <NavDropdown.Item>Orders</NavDropdown.Item>
                  </LinkContainer>
                </NavDropdown>
              )}
            </Nav>
....
```

### List Orders For Admin
update in **orderController.js** <br/>
**orderController.js** <br/>
```
.....
//@desc Get all orders
//@route GET /api/orders
//@access Private/Admin
const getOrders = asyncHandler(async(req, res) => {
    const orders = await Order.find({}).populate('user', 'id name');
    res.status(200).json(orders);
});
....
```

update in **ordersApiSlice.js** <br/>
```
....
getOrders: builder.query ({
            query: () => ({
                url: ORDERS_URL,
            }),
            keepUnusedDataFor: 5,
        })
....
    useGetMyOrdersQuery,
    useGetOrdersQuery,
} = ordersApiSlice;
```

update in **OrderListScreen.js** <br/>
```
import React from 'react';
import {LinkContainer, ListContainer} from 'react-router-bootstrap';
import {Table, Button} from 'react-bootstrap';
import { FaTimes } from 'react-icons/fa';
import Message from '../../components/Message';
import Loader from '../../components/Loader';
import { useGetOrdersQuery } from '../../slices/ordersApiSlice';

const OrderListScreen = () => {
  const {data: orders, isLoading, error} = useGetOrdersQuery();

  return (
    <>
      <h1>Orders</h1>
      {isLoading ? (
        <Loader />
      ) : error ? (
        <Message variant='danger'>{error}</Message>
      ) : (
        <Table striped hover responsive className='table-sm'>
          <thead>
            <tr>
              <th>ID</th>
              <th>USER</th>
              <th>DATE</th>
              <th>TOTAL</th>
              <th>PAID</th>
              <th>DELIVERED</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            {orders.map((order) => (
              <tr key={order._id}>
                <td>{order._id}</td>
                <td>{order.user && order.user.name}</td>
                <td>{order.createdAt.substring(0,10)}</td>
                <td>{order.totalPrice}</td>
                <td>
                  {order.isPaid ? (
                    order.paidAt.substring(0, 10)
                  ) : (
                    <FaTimes style={{color: 'red'}} />
                  )}
                </td>
                <td>
                  {order.isDelivered ? (
                    order.deliveredAt.substring(0, 10)
                  ) : (
                    <FaTimes style={{color: 'red'}} />
                  )}
                </td>
                <td>
                  <LinkContainer to={`/order/${order._id}`}>
                    <Button variant='light' className='btn-sm'>Details</Button>
                  </LinkContainer>
                </td>
              </tr>
            ))}
          </tbody>
        </Table>
      )}
    </>
  )
}

export default OrderListScreen
```

### Delivered Order Status
update in **OrderController.js** <br />
```
//@desc update order to delivered
//@route PUT /api/orders/:id/deliver
//@access Private/Admin
const updateOrderToDelivered = asyncHandler(async(req, res) => {
    const order = await Order.findById(req.params.id);
    if(order) {
        order.isDelivered = true;
        order.deliveredAt = Date.now();

        const updatedOrder = await order.save();
        res.status(200).json(updatedOrder);
    } else {
        res.status(404);
        throw new Error('Order not found');
    }
});
```

update in **ordersApiSlice.js** <br/>
```
}),
        deliverOrder: builder.mutation({
            query: (orderId) => ({
                url: `${ORDERS_URL}/${orderId}/deliver`,
                method: 'PUT'
            }),
        })
....

useDeliverOrderMutation,
} = ordersApiSlice;
```

update in **OrderScreen.js** <br />
```
....
import { useGetOrderDetailsQuery, usePayOrderMutation, useGetPayPalClientIdQuery, useDeliverOrderMutation } from '../slices/ordersApiSlice';
....
const [payOrder, {isLoading:loadingPay}] = usePayOrderMutation();
const [deliverOrder, {isLoading:loadingDeliver}] = useDeliverOrderMutation();
const [{isPending}, paypalDispatch] = usePayPalScriptReducer();
.....
    const deliverOrderHandler = async() => {
        try {
            await deliverOrder(orderId);
            refetch();
            toast.success('Order delivered');
        } catch (err) {
            toast.error(err?.data?.message || err.message);
        }
    }
.....
</ListGroup.Item>
)}
{loadingDeliver && <Loader />}
{userInfo.isAdmin && order.isPaid && !order.isDelivered && (
<ListGroup.Item>
  <Button type='button' className='btn btn-block' onClick={deliverOrderHandler}> Mark As Delivered</Button>
</ListGroup.Item>
)}
</ListGroup>
</Card>
....
```

### List Products For Admin
create new file **ProductListScreen.js** under frontend/screens/admin <br />
**ProductListScreen.js** <br />
```
import React from 'react';
import { LinkContainer } from 'react-router-bootstrap';
import {Table, Button, Row, Col} from 'react-bootstrap';
import { FaTimes, FaEdit, FaTrash } from 'react-icons/fa';
import Message from '../../components/Message';
import Loader from '../../components/Loader';
import { useGetProductsQuery } from '../../slices/productsApiSlice';

const ProductListScreen = () => {
    const {data:products, isLoading, error} = useGetProductsQuery();
    
    const deleteHandler = (id) => {
        console.log('delete', id)
    }

  return (
    <>
        <Row className='align-items-center'>
            <Col>
                <h1>Products</h1>
            </Col>
            <Col className='text-end'>
                <Button className='btn-sm m-3'>
                    <FaEdit />Create Product
                </Button>
            </Col>
        </Row>

        {isLoading ? <Loader /> : error ? <Message variant='danger'>{error}</Message>
            : (
                <>
                    <Table>
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>NAME</th>
                                <th>PRICE</th>
                                <th>CATEGORY</th>
                                <th>BRAND</th>
                                <th></th>
                            </tr>
                        </thead>
                        <tbody>
                            {products.map((product) => (
                                <tr key={product._id}>
                                    <td>{product._id}</td>
                                    <td>{product.name}</td>
                                    <td>{product.price}</td>
                                    <td>{product.category}</td>
                                    <td>{product.brand}</td>
                                    <td>
                                        <LinkContainer to={`/admin/product/${product._id}/edit`}>
                                            <Button variant='light' className='btn-sm mx-2'>
                                                <FaEdit />
                                            </Button>
                                        </LinkContainer>
                                        <Button
                                            variant='danger' className='btn-sm'
                                            onClick={() => deleteHandler(product._id)}>
                                            <FaTrash style={{color: 'white'}}/>
                                        </Button>
                                    </td>
                                </tr>
                            ))}
                        </tbody>
                    </Table>
                </>
            )       
        }  
    </>
)}

export default ProductListScreen
```

update in **index.js**
```
....
import ProductListScreen from './screens/admin/ProductListScreen.js';
....
<Route path='' element={<AdminRoute />}>
        <Route path='/admin/orderlist' element={<OrderListScreen />} />
        <Route path='/admin/productlist' element={<ProductListScreen />} />
       </Route>
    </Route>
```

### Creating Products
update in **productController.js** <br />
```
......
......
        throw new Error('Resource not found');
    }
});

//@desc Create a product
//@route POST /api/products
//@access Private/Admin
const createProduct = asyncHandler(async(req, res) => {
    const product = new Product ({
        name: 'Sample name',
        price: 0,
        user: req.user._id,
        image: '/images/sample.jpg',
        brand: 'Sample brand',
        category: 'Sample category',
        countInStock: 0,
        numReviews: 0,
        description: 'Sample description',
    })
    const createProduct = await product.save();
    res.status(200).json(createProduct);
});

export {getProducts, getProductById, createProduct};
```

update in **productRoutes.js** <br />
```
import express, { Router } from "express";
const router = express.Router();
import { getProducts, getProductById, createProduct } from "../controllers/productController.js";
import {protect, admin} from '../middleware/authMiddleware.js';

router.route('/').get(getProducts).post(protect, admin, createProduct);
router.route('/:id').get(getProductById);

export default router;
```

### Edit Product
update in **productController.js** <br />
**productController.js** <br/>
```
//@desc Update a product
//@route PUT /api/products/:id
//@access Private/Admin
const updateProduct = asyncHandler(async(req, res) => {
    const {name, price, description, image, brand, category, countInStock} = req.body;
    const product = await Product.findById(req.params.id);
    if(product) {
        product.name = name;
        product.price = price;
        product.description = description;
        product.image = image;
        product.brand = brand;
        product.category = category;
        product.countInStock = countInStock;
        
        const updatedProduct = await product.save();
        res.json(updatedProduct);
    } else {
        res.status(404);
        throw new Error('Resouce not found');
    }
});

export {getProducts, getProductById, createProduct, updateProduct};
```

update in **ProductRoutes.js** <br />
**ProductRoutes.js** <br />
```
....
import { getProducts, getProductById, createProduct, updateProduct } from "../controllers/productController.js";
....
router.route('/:id').get(getProductById).put(protect, admin, updateProduct);

export default router;
```

update in **productsApiSlice.js** <br />
**productsApiSlice.js** <br />
```
getProducts: builder.query({
            query:() => ({
                url: PRODUCTS_URL,
            }),
            providesTags: ['Product'],
            keepUnusedDataFor: 5,
        }),
.....
.....
updateProduct: builder.mutation ({
            query: (data) => ({
                url: `${PRODUCTS_URL}/${data.productId}`,
                method: 'PUT',
                body: data,
            }),
            invalidatesTags: ['Product'],
        })
    }),
})

export const {useGetProductsQuery, 
    useGetProductDetailsQuery, 
    useCreateProductMutation,
    useUpdateProductMutation
} = productsApiSlice;
```

create new file **ProductEditScreen.js** <br/>
**ProductEditScreen.js** <br/>
```
import React from 'react';
import { useState, useEffect } from 'react';
import {Link, useNavigate, useParams} from 'react-router-dom';
import {Form, Button} from 'react-bootstrap';
import Message from '../../components/Message';
import Loader from '../../components/Loader';
import FormContainer from '../../components/FormContainer';
import {toast} from 'react-toastify';
import { useUpdateProductMutation, useGetProductDetailsQuery } from '../../slices/productsApiSlice';

const ProductEditScreen = () => {
    const {id: productId} = useParams();
    const [name, setName] = useState('');
    const [price, setPrice] = useState(0);
    const [image, setImage] = useState('');
    const [brand, setBrand] = useState('');
    const [category, setCategory] = useState('');
    const [countInStock, setCountInStock] = useState(0);
    const [description, setDescription] = useState('');

    const {
        data: product,
        isLoading,
        refetch,
        error
    } = useGetProductDetailsQuery(productId);
    
    const [updateProduct, {isLoading: loadingUpdate}] = useUpdateProductMutation();
    const navigate = useNavigate();

    useEffect(() => {
        if(product) {
            setName(product.name);
            setPrice(product.price);
            setImage(product.image);
            setBrand(product.brand);
            setCategory(product.category);
            setCountInStock(product.countInStock);
            setDescription(product.description);
        }
    }, [product]);

    const submitHandler = async (e) => {
        e.preventDefault();
        const updatedProduct = {
            productId,
            name,
            price,
            image,
            brand,
            category,
            countInStock,
            description
        };

        const result = await updateProduct(updatedProduct);
        if(result.error) {
            toast.error(result.error);
        } else {
            toast.success('Product updated');
            navigate('/admin/productlist');
        }
    }

  return (
    <>
        <Link to='/admin/productlist' className='btn btn-light my-3'>Go Back</Link>
        <FormContainer>
            <h1>Edit Product</h1>
            {loadingUpdate && <Loader />}
            {isLoading ? <Loader /> : error ? <Message variant='danger'>{error}</Message>
            : (
                <Form onSubmit={submitHandler}>
                    <Form.Group controlId='name' className='my-2'>
                        <Form.Label>Name</Form.Label>
                        <Form.Control 
                            type='text'
                            placeholder='Enter name'
                            value={name}
                            onChange={(e) => setName(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Form.Group controlId='price' className='my-2'>
                        <Form.Label>Price</Form.Label>
                        <Form.Control 
                            type='number'
                            placeholder='Enter price'
                            value={price}
                            onChange={(e) => setPrice(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    {/* Image input placeholder*/}
                    <Form.Group controlId='brand' className='my-2'>
                        <Form.Label>Brand</Form.Label>
                        <Form.Control 
                            type='text'
                            placeholder='Enter brand'
                            value={brand}
                            onChange={(e) => setBrand(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Form.Group controlId='countInStock' className='my-2'>
                        <Form.Label>Count In Stock</Form.Label>
                        <Form.Control 
                            type='number'
                            placeholder='Enter countInStock'
                            value={countInStock}
                            onChange={(e) => setCountInStock(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Form.Group controlId='category' className='my-2'>
                        <Form.Label>Category</Form.Label>
                        <Form.Control 
                            type='text'
                            placeholder='Enter category'
                            value={category}
                            onChange={(e) => setCategory(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Form.Group controlId='description' className='my-2'>
                        <Form.Label>Description</Form.Label>
                        <Form.Control 
                            type='text'
                            placeholder='Enter description'
                            value={description}
                            onChange={(e) => setDescription(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Button type='submit' variant='primary' className='my-2'>
                        Update
                    </Button>
                </Form>
            )}
        </FormContainer>
    </>
  )
}

export default ProductEditScreen
```

update in **index.js** <br />
```
.....
import ProductEditScreen from './screens/admin/ProductEditScreen.js';
.....
<Route path='/admin/product/:id/edit' element={<ProductEditScreen />} />
       </Route>
    </Route>
....
```

### Update Product Bug fix
update in **productApiSlice.js** <br />
```
.....
getProducts: builder.query({
            query:() => ({
                url: PRODUCTS_URL,
            }),
            providesTags: ['Products'],
            keepUnusedDataFor: 5,
}),
.....
.....
updateProduct: builder.mutation ({
            query: (data) => ({
                url: `${PRODUCTS_URL}/${data.productId}`,
                method: 'PUT',
                body: data,
            }),
            invalidatesTags: ['Products'],
        })
....
```

### Multer & Image Upload Endpoint
create new file **uploadRoutes.js** under backend/routes <br />
**uploadRoutes.js** <br/>
```
import path from 'path';
import express from 'express';
import multer from 'multer';
const router = express.Router();

const storage = multer.diskStorage({
    destination(req, file, cb) {
        cb(null, 'uploads/');
    },
    filename(req, file, cb) {
        cb(null, `${file.fieldname}-${Date.now()}$path.extname(file.originalname)`);
    }
});

function checkFileTypes(file, cb) {
    const filetypes = /jpg|jpeg|png/;
    const extname = filetypes.test(path.extname(file.originalname).toLowerCase());
    const mimetype = filetypes.test(file.minetype);
    if(extname && mimetype) {
        return cb(null, true);
    } else {
        cb('Image only!');
    }
}
    
const upload = multer ({
    storage,
});
router.post('/', upload.single('image'),(req, res) => {
    res.send({
        message: 'Image Upload',
        image: `/${req.file.path}`
    });
})

export default router;
```

update in **server.js** <br />
```
....
import uploadRoutes from './routes/uploadRoutes.js';
.....
.....
app.use('/api/orders', orderRoutes);
app.use('/api/upload', uploadRoutes);

const __dirname = path.resolve(); //set __dirname to current directory
app.use('/uploads', express.static(path.join(__dirname, '/uploads')));

app.use(notFound);
....
```

### Upload Product Image - Frontend
update in **constants.js** <br />
```
....
export const UPLOAD_URL = '/api/upload';
```

update in **productsApiSlice.js** <br />
```
import { PRODUCTS_URL, UPLOAD_URL } from "../constants";
....
uploadProductImage: builder.mutation ({
            query: (data) => ({
                url: `${UPLOAD_URL}`,
                method: 'POST',
                body: data,
            })
        })
.....
useUploadProductImageMutation
} = productsApiSlice;
```

update in **productEditScreen.js** <br />
**productEditScreen.js** <br/>
```
import React from 'react';
import { useState, useEffect } from 'react';
import {Link, useNavigate, useParams} from 'react-router-dom';
import {Form, Button} from 'react-bootstrap';
import Message from '../../components/Message';
import Loader from '../../components/Loader';
import FormContainer from '../../components/FormContainer';
import {toast} from 'react-toastify';
import { useUpdateProductMutation, useGetProductDetailsQuery, useUploadProductImageMutation } from '../../slices/productsApiSlice';

const ProductEditScreen = () => {
    const {id: productId} = useParams();
    const [name, setName] = useState('');
    const [price, setPrice] = useState(0);
    const [image, setImage] = useState('');
    const [brand, setBrand] = useState('');
    const [category, setCategory] = useState('');
    const [countInStock, setCountInStock] = useState(0);
    const [description, setDescription] = useState('');

    const {
        data: product,
        isLoading,
        refetch,
        error
    } = useGetProductDetailsQuery(productId);
    
    const [updateProduct, {isLoading: loadingUpdate}] = useUpdateProductMutation();

    const [uploadProductImage, {isLoading: loadingUpload}] = useUploadProductImageMutation();

    const navigate = useNavigate();

    useEffect(() => {
        if(product) {
            setName(product.name);
            setPrice(product.price);
            setImage(product.image);
            setBrand(product.brand);
            setCategory(product.category);
            setCountInStock(product.countInStock);
            setDescription(product.description);
        }
    }, [product]);

    const submitHandler = async (e) => {
        e.preventDefault();
        const updatedProduct = {
            productId,
            name,
            price,
            image,
            brand,
            category,
            countInStock,
            description,
        };
    
        const result = await updateProduct(updatedProduct);
        if(result.error) {
            toast.error(result.error);
            refetch();
        } else {
            toast.success('Product updated');
            navigate('/admin/productlist');
        }
    }

    const uploadFileHandler = async(e) => {
        const formData = new FormData();
        formData.append('image', e.target.files[0]);
        try {
            const res = await uploadProductImage(formData).unwrap();
            toast.success(res.message);
            setImage(res.image);
        } catch (err) {
            toast.error(err?.data?.message || err.error);
        }
    }

  return (
    <>
        <Link to='/admin/productlist' className='btn btn-light my-3'>Go Back</Link>
        <FormContainer>
            <h1>Edit Product</h1>
            {loadingUpdate && <Loader />}
            {isLoading ? <Loader /> : error ? <Message variant='danger'>{error}</Message>
            : (
                <Form onSubmit={submitHandler}>
                    <Form.Group controlId='name' className='my-2'>
                        <Form.Label>Name</Form.Label>
                        <Form.Control 
                            type='text'
                            placeholder='Enter name'
                            value={name}
                            onChange={(e) => setName(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Form.Group controlId='price' className='my-2'>
                        <Form.Label>Price</Form.Label>
                        <Form.Control 
                            type='number'
                            placeholder='Enter price'
                            value={price}
                            onChange={(e) => setPrice(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Form.Group controlId='image' className='my-2'>
                        <Form.Label>Images</Form.Label>
                        <Form.Control type="text" placeholder='Enter Image ul'
                            value={image} onChange={(e) => setImage}>
                        </Form.Control>
                        <Form.Control type="file" label='Choose file'
                            onChange={uploadFileHandler}>
                        </Form.Control>
                    </Form.Group>
                    <Form.Group controlId='brand' className='my-2'>
                        <Form.Label>Brand</Form.Label>
                        <Form.Control 
                            type='text'
                            placeholder='Enter brand'
                            value={brand}
                            onChange={(e) => setBrand(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Form.Group controlId='countInStock' className='my-2'>
                        <Form.Label>Count In Stock</Form.Label>
                        <Form.Control 
                            type='number'
                            placeholder='Enter countInStock'
                            value={countInStock}
                            onChange={(e) => setCountInStock(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Form.Group controlId='category' className='my-2'>
                        <Form.Label>Category</Form.Label>
                        <Form.Control 
                            type='text'
                            placeholder='Enter category'
                            value={category}
                            onChange={(e) => setCategory(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Form.Group controlId='description' className='my-2'>
                        <Form.Label>Description</Form.Label>
                        <Form.Control 
                            type='text'
                            placeholder='Enter description'
                            value={description}
                            onChange={(e) => setDescription(e.target.value)}
                        ></Form.Control>
                    </Form.Group>
                    <Button type='submit' variant='primary' className='my-2'>
                        Update
                    </Button>
                </Form>
            )}
        </FormContainer>
    </>
  )
}

export default ProductEditScreen
```

### Delete Products
update in **productController.js** <br />
```
.....

//@desc Delete a product
//@route Delete /api/products/:id
//@access Private/Admin
const deleteProduct = asyncHandler(async (req, res) => {
    const product = await Product.findById(req.params.id);
    if(product) {
        await Product.deleteOne({_id: product._id});
        res.status(200).json({message: 'Product deleted'});
    } else {
        res.status(404);
        throw new Error('Resouce not found');
    }
});

export {getProducts, getProductById, createProduct, updateProduct, deleteProduct};
```

update in **productRoutes.js** <br />
```
.....
import { getProducts, getProductById, createProduct, updateProduct, deleteProduct } from "../controllers/productController.js";
.....
router.route('/:id').get(getProductById).put(protect, admin, updateProduct).delete(protect, admin, deleteProduct);
....
```

update in productApliSlice.js <br />
```
deleteProduct: builder.mutation({
            query: (productId) => ({
                url: `${PRODUCTS_URL}/${productId}`,
                method: 'DELETE',
            })
        })
....
useDeleteProductMutation,
} = productsApiSlice;
```

update in **ProductListScreen.js** <br />
```
....
import { useGetProductsQuery, useCreateProductMutation, useDeleteProductMutation } from '../../slices/productsApiSlice';
....
....
const [deleteProduct, {isLoading: loadingDelete}] = useDeleteProductMutation();
.....
.....
const deleteHandler = async (id) => {
        if(window.confirm('Are you sure')) {
            try {
                await deleteProduct(id);
                toast.success('Product deleted');
                refetch();
            } catch(err) {
                toast.error(err?.data?.message || err.error);
            }
        }
    }
.....
.....
{loadingCreate && <Loader />}
{loadingDelete && <Loader />}
....
```

### 
update in **userController.js** <br />
```
import asyncHandler from '../middleware/asyncHandler.js';
import User from '../models/userModel.js';
import generateToken from '../utils/generateToken.js';

//@desc Auth user & get token
//@route POST /api/users/login
//@access Public
const authUser = asyncHandler(async (req, res) => {
    const {email, password} = req.body;
    const user = await User.findOne({email});
    if(user && (await user.matchPassword(password))) {
        generateToken(res, user._id);
        res.status(200).json({
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
        generateToken(res, user._id);
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
            name: updatedUser.name,
            email: updatedUser.email,
            isAdmin: updatedUser.isAdmin
        });
    } else {
        res.status(404);
        throw new Error('user not found');
    }
});

//@desc Get users
//@route GET /api/users
//@access Private/Admin
const getUsers = asyncHandler(async (req, res) => {
    const users = await User.find({});
    res.status(200).json(users);
});

//@desc Get user by ID
//@route GET /api/users/:id
//@access Private/Admin
const getUserByID = asyncHandler(async (req, res) => {
    const user = await User.findById(req.params.id).select('-password');
    if(user) {
        res.status(200).json(user);
    } else {
        res.status(404);
        throw new Error('User not found');
    }
});

//@desc Delete users
//@route DELETE /api/users/:id
//@access Private/Admin
const deleteUser = asyncHandler(async (req, res) => {
    const user = await User.findById(req.params.id);
    if(user) {
        if(user.isAdmin) {
            res.status(400);
            throw new Error('Cannot delete admin user');
        } 
        await User.deleteOne({_id: user._id})
        res.status(200).json({message: 'User deleted successfully'});
    } else {
        res.status(404);
        throw new Error('User not found');
    }
});

//@desc Update user
//@route PUT /api/users/:id
//@access Private/Admin
const updateUser = asyncHandler(async (req, res) => {
    const user = await User.findById(req.params.id);
    if(user) {
        user.name = req.body.name || user.name;
        user.email = req.body.email || user.email;
        user.isAdmin = Boolean(req.body.isAdmin);

        const updatedUser = await user.save();

        res.status(200).json({
            _id: updatedUser._id,
            name: updatedUser.name,
            email: updatedUser.email,
            isAdmin: updatedUser.isAdmin,
        });
    } else {
        res.status(404);
        throw new Error('User not found');
    }
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

### List Users For Admin
update in **usersApiSlice.js** <br />
```
.....
getUsers: builder.query({
            query: () => ({
                url: USERS_URL
            }),
            providesTags: ['Users'],
            keepUnusedDataFor: 5
        })
....
    useGetUsersQuery
} = usersApiSlice;
```

create new file **UserListScreen.js** under frontend/screens/admin <br />
**UserListScreen.js** <br />
```
import React from 'react';
import { LinkContainer } from 'react-router-bootstrap';
import { Table, Button } from 'react-bootstrap';
import {FaTimes, FaTrash, FaEdit, FaCheck} from 'react-icons/fa';
import Message from '../../components/Message';
import Loader from '../../components/Loader';
import { useGetUsersQuery } from '../../slices/usersApiSlice';

const UserListScreen = () => {
    const {data: users, refetch, isLoading, error} = useGetUsersQuery();

    const deleteHandler = (id) => {
        console.log('delete');
    }

    return (
        <>
            <h1>Users</h1>
            {isLoading ? (
                <Loader />
            ) : error ? (
                <Message variant='danger'>{error}</Message>
            ) : (
                <Table striped hover responsive className='table-sm'>
                    <thread>
                        <tr>
                            <th>ID</th>
                            <th>NAME</th>
                            <th>EMAIL</th>
                            <th>ADMIN</th>
                            <th></th>
                        </tr>
                    </thread>
                    <tbody>
                        {users.map((user) => (
                            <tr key={user._id}>
                                <td>{user._id}</td>
                                <td>{user.name}</td>
                                <td><a href={`mailto:${user.email}`}>{user.email}</a></td>
                                <td>
                                    {user.isAdmin ? (
                                        <FaCheck style={{color: 'green'}} />
                                    ) : (
                                        <FaTimes style={{color: 'red' }} />
                                    )}
                                </td>
                                <td>
                                    <LinkContainer to={`/admin/user/${user._id}/edit`}>
                                        <Button variant='light' className='btn-sm'>
                                            <FaEdit />
                                        </Button>
                                    </LinkContainer>
                                    <Button
                                        variant='danger'
                                        className='btn-sm'
                                        onClick={() => deleteHandler(user._id)}
                                    >
                                        <FaTrash style={{color: 'white'}} />
                                    </Button>
                                </td>
                            </tr>
                        ))}
                    </tbody>
                </Table>
            )}
        </>
    )
}

export default UserListScreen
```

update in **index.js** <br />
```
....
import UserListScreen from './screens/admin/UserListScreen.js';
.....
<Route path='/admin/userlist' element={<UserListScreen />} />
....
```

### Delete Users
update in **usersApiSlice.js** <br />
```
deleteUser: builder.mutation ({
            query: (userId) => ({
                url: `${USERS_URL}/${userId}`,
                method: 'DELETE',
            }),
        })
....
  useDeleteUserMutation
} = usersApiSlice;
```

update in **UserListScreen.js** <br />
```
.....
import Loader from '../../components/Loader';
import { toast } from 'react-toastify';
import { useGetUsersQuery, useDeleteUserMutation } from '../../slices/usersApiSlice';

const UserListScreen = () => {
    const {data: users, refetch, isLoading, error} = useGetUsersQuery();
    const [deleteUser, {isLoading: loadingDelete}] = useDeleteUserMutation();

    const deleteHandler = async (id) => {
        if(window.confirm('Are you sure?')) {
            try {
                await deleteUser(id);
                toast.success('User deleted');
                refetch();
            } catch (err) {
                toast.error(err?.data?.message || err.error);
            }
        }
    }

    return (
        <>
            <h1>Users</h1>
            {loadingDelete && <Loader />}
            {isLoading ? (
.....
.....
```

### Update Users
update in **usersApiSlice.js** <br/>
```
.....
.....
getUserDetails: builder.query({
            query: (userId) => ({
                url: `${USERS_URL}/${userId}`,
            }),
            keepUnusedDataFor: 5,
        }),
        updateUser: builder.mutation ({
            query: (data) => ({
                url: `${USERS_URL}/${data.userId}`,
                method: 'PUT',
                body: data,
            }),
            invalidatesTags: ['Users'],
        })
....
    useGetUserDetailsQuery,
    useUpdateUserMutation
} = usersApiSlice;
```

create new file **UserEditScreen.js** under frontend/screens/admin <br />
**UserEditScreen.js** <br />
```
import React from 'react';
import {useState, useEffect} from 'react';
import {Link, useNavigate, useParams} from 'react-router-dom';
import {Form, Button} from 'react-bootstrap';
import Message from '../../components/Message';
import Loader from '../../components/Loader';
import FormContainer from '../../components/FormContainer';
import {toast} from 'react-toastify';
import { useUpdateUserMutation, useGetUserDetailsQuery } from '../../slices/usersApiSlice';

const UserEditScreen = () => {
    const {id: userId} = useParams();

    const [name, setName] = useState('');
    const [email, setEmail] = useState('');
    const [isAdmin, setIsAdmin] = useState(false);

    const {
        data: user,
        isLoading,
        refetch,
        error,
    } = useGetUserDetailsQuery(userId);

    const [updateUser, {isLoading: loadingUpdate}] = useUpdateUserMutation();
    const navigate = useNavigate();
    
    useEffect(() => {
        if(user) {
            setName(user.name);
            setEmail(user.email);
            setIsAdmin(user.isAdmin);
        }
    }, [user]);

    const submitHandler = async(e) => {
        e.preventDefault();
        try {
            await updateUser({userId, name, email, isAdmin});
            toast.success('User updated successfully');
            refetch();
            navigate('/admin/userlist');
        } catch (err) {
            toast.error(err?.data?.message || err.error);
        }
    }

    return (
        <>
            <Link to='/admin/userlist' className='btn btn-light my-3'>Go Back</Link>
            <FormContainer>
                <h1>Edit User</h1>
                {loadingUpdate && <Loader />}
                {isLoading ? <Loader /> : error ? <Message variant='danger'>{error}</Message>
                : (
                    <Form onSubmit={submitHandler}>
                        <Form.Group controlId='name' className='my-2'>
                            <Form.Label>Name</Form.Label>
                            <Form.Control 
                                type='text'
                                placeholder='Enter name'
                                value={name}
                                onChange={(e) => setName(e.target.value)}
                            ></Form.Control>
                        </Form.Group>
                        <Form.Group controlId='email' className='my-2'>
                            <Form.Label>Email</Form.Label>
                            <Form.Control 
                                type='email'
                                placeholder='Enter email'
                                value={email}
                                onChange={(e) => setEmail(e.target.value)}
                            ></Form.Control>
                        </Form.Group>
                        <Form.Group controlId='isAdmin' className='my-2'>
                            <Form.Check
                                type='checkbox'
                                label='Is Admin'
                                checked={isAdmin}
                                onChange={(e) => setIsAdmin(e.target.checked)}
                            >
                            </Form.Check>
                        </Form.Group>
                        <Button type='submit' variant='primary' className='my-2'>
                            Update
                        </Button>
                    </Form>
                )}
            </FormContainer>
        </>
      )
}

export default UserEditScreen
```

update in **index.js** <br />
```
.....
import UserEditScreen from './screens/admin/UserEditScreen.js';
.....
 <Route path='/admin/user/:id/edit' element={<UserEditScreen />} />
....
```

## Reviews, Search and More
### Create Reviews - Backend
```
....
....
//@desc Create a new review
//@route POST /api/products/:id/review
//@access Private
const createProductReview = asyncHandler(async (req, res) => {
    const {rating, comment} = req.body;
    const product = await Product.findById(req.params.id);
    if(product) {
        const alreadyReviewed = product.reviews.find(
            (review) => review.user.toString() === req.user._id.toString()
        );
        if(alreadyReviewed) {
            res.status(400);
            throw new Error('Product already reviewed');
        }
        const review = {
            name: req.user.name,
            rating: Number(rating),
            comment,
            user: req.user._id,
        };
        product.reviews.push(review);
        product.numReviews = product.reviews.length;
        product.rating = product.reviews.reduce((acc, review) => acc + review.rating, 0) / product.reviews.length;
        await product.save();
        res.status(201).json({message: 'Review added'});
    } else {
        res.status(404);
        throw new Error('Resouce not found');
    }
});

export {getProducts, getProductById, createProduct, updateProduct, deleteProduct, createProductReview};
```

update in **productRoutes.js**
```
.....
import { getProducts, getProductById, createProduct, updateProduct, deleteProduct, createProductReview } from "../controllers/productController.js";
....
....
router.route('/:id/reviews').post(protect, createProductReview);
....
```

### Create Reviews - Frontend
update in **productsApiSlice.js** <br/>
```
.....
createReview: builder.mutation({
            query: (data) => ({
                url: `${PRODUCTS_URL}/${data.productId}/reviews`,
                method: 'POST',
                body: data,
            }),
            invalidatesTags: ['Product'],
        })
    }),
})
....
....
    useCreateReviewMutation
} = productsApiSlice;
```

update in **ProductScreen.js** <br/>
```
import React, { useState } from 'react';
import { useParams, useNavigate } from 'react-router-dom';
import { Link } from 'react-router-dom';
import {Form, Row, Col, Image, ListGroup, Card, Button} from 'react-bootstrap';
import { useDispatch, useSelector } from 'react-redux';
import {toast} from 'react-toastify';
import Rating from '../components/Rating';
import Loader from '../components/Loader.js';
import Message from '../components/Message';
import { useGetProductDetailsQuery, useCreateReviewMutation } from '../slices/productsApiSlice';
import { addToCart } from '../slices/cartSlice';

const ProductScreen = () => {
  const {id:productId} = useParams();
  const dispatch = useDispatch();
  const navigate = useNavigate();

  const [qty, setQty] = useState(1);
  const [rating, setRating] = useState(0);
  const [comment, setComment] = useState('');

  const {data:product, isLoading, refetch, error} = useGetProductDetailsQuery(productId);

  const [createReview, {isLoading: loadingProductReview}] = useCreateReviewMutation();
  
  const {userInfo} = useSelector((state) => state.auth);
  
  const addToCartHandler = () => {
    dispatch(addToCart({...product, qty}));
    navigate('/cart');
  };

  const submitHandler = async (e) => {
    e.preventDefault();
    try {
      await createReview({
        productId,
        rating,
        comment
      }).unwrap();
      refetch();
      toast.success('Review Submitted');
    } catch (err) {
      toast.error(err?.data?.message || err.error);
    }
  }

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
        <>
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
          <Row className='review'>
            <Col md={6}>
              <h2>Reviews</h2>
              {product.reviews.length === 0 && <Message>No Reviews</Message>}
              <ListGroup variant='flush'>
                {product.reviews.map((review) => (
                  <ListGroup.Item key={review._id}>
                    <strong>{review.name}</strong>
                    <Rating value={review.rating} />
                    <p>{review.createdAt.substring(0, 10)}</p>
                    <p>{review.comment}</p>
                  </ListGroup.Item>
                ))}
                <ListGroup.Item>
                  <h2>Write a Customer Review</h2>
                  {loadingProductReview && <Loader />}
                  {userInfo ? (
                    <Form onSubmit={submitHandler}>
                      <Form.Group controlId='rating' className='my-2'>
                        <Form.Label>Rating</Form.Label>
                        <Form.Control
                          as='select'
                          value={rating}
                          onChange={(e) => setRating(e.target.value)}
                        >
                          <option value=''>Select...</option>
                          <option value='1'>1 - Poor</option>
                          <option value='2'>2 - Fair</option>
                          <option value='3'>3 - Good</option>
                          <option value='4'>4 - Very Good</option>
                          <option value='5'>5 - Excellent</option>
                        </Form.Control>
                      </Form.Group>
                      <Form.Group controlId='comment' className='my-2'>
                        <Form.Label>comment</Form.Label>
                        <Form.Control
                          as='textarea'
                          row='3'
                          value={comment}
                          onChange={(e) => setComment(e.target.value)}
                        ></Form.Control>
                      </Form.Group>
                      <Button
                        disabled={loadingProductReview}
                        type='submit'
                        variant='primary'
                      >
                        Submit
                      </Button>
                    </Form>
                  ) : (
                    <Message>
                      Please <Link to='/login'>Sign in</Link> to write a review{' '}
                    </Message>
                  ) }
                </ListGroup.Item>
              </ListGroup>                              
            </Col>
          </Row>
        </>
      )}
    </>
  )
}

export default ProductScreen
```

### Paginate Products
update in **ProductController.js** <br/>
```
//@desc Fetch all products
//@route GET /api/products
//@access Public
const getProducts = asyncHandler(async (req, res) => {
    const pageSize = 4;
    const page = Number(req.query.pageNumber) || 1;
    const count = await Product.countDocuments();

    const products = await Product.find({})
        .limit(pageSize)
        .skip(pageSize * (page - 1));
    res.json({products, page, pages: Math.ceil(count/pageSize)});
});
......
......
```

update in **HomeScreen.js** <br/>
```
.....
const {data, isLoading, error} = useGetProductsQuery();
.....
.....
{data.products.map((product) => (
.....
```

update in **productsApiSlice.js** <br />
```
.....
getProducts: builder.query({
            query:({pageNumber}) => ({
                url: PRODUCTS_URL,
                params: {
                    pageNumber,
                },
            }),
            providesTags: ['Products'],
            keepUnusedDataFor: 5,
        }),
.....
.....
```

update in **HomeScreen.js** <br />
```
import { useParams } from 'react-router-dom';
import { useGetProductsQuery } from '../slices/productsApiSlice';

const HomeScreen = () => {
  const {pageNumber} = useParams();

  const {data, isLoading, error} = useGetProductsQuery({pageNumber});
....
....
```

update in **index.js** <br />
```
<Route path='/page/:pageNumber' element={<HomeScreen />} />
```

### Paginate Component
create new file **Paginate.js** under frontend/components <br />
```
import { Pagination } from "react-bootstrap";
import { LinkContainer } from "react-router-bootstrap";

const Paginate = ({pages, page, isAdmin = false}) => {
    return (
        pages > 1 && (
            <Pagination>
                {[...Array(pages).keys()].map((x) => (
                    <LinkContainer 
                        key={x + 1}
                        to = {
                            !isAdmin
                            ? `/page/${x + 1}`
                            : `/admin/productlist/${x + 1}`
                        }
                    >
                    <Pagination.Item active={x + 1 === page}>{x + 1}</Pagination.Item>
                    </LinkContainer>
                ))}
            </Pagination>
        )
    )
}

export default Paginate;
```

update in HomeScreen.js <br />
```
.....
import Paginate from '../components/Paginate';
......
......
<Paginate pages={data.pages} page={data.page} />
......
```

update in **ProductListScreen.js** <br />
```
.....
import { useParams } from 'react-router-dom';
.....
const {pageNumber} = useParams();
const {data, isLoading, error, refetch} = useGetProductsQuery({pageNumber});
.....
<tbody>
{data.products.map((product) => (
.....
```

update in **index.js** <br />
```
<Route path='/admin/productlist/:pageNumber' element={<ProductListScreen />} />
```

update in **ProductListScreen.js** <br />
```
.....
import Paginate from '../../components/Paginate';
.....
.....
<Paginate pages={data.pages} page={data.page} isAdmin={true} />
......
