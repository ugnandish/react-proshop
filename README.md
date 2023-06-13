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
**node_modules<br/>
.env**

install react-bootstrap and icons <br/>
**npm i react-bootstrap bootstrap react-icons**

bootstrap file add in index.js file <br/>
**import 'bootstrap/dist/css/bootstrap.min.css'**

create **components** folder under **src** <br/>
create **Header.js** file under **components** <br/>
create **Footer.js** file under **components**

## Lists Products
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
]
etc....
export default products;
```


 
