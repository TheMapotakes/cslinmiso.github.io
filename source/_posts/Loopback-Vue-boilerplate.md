title: "Loopback-Vue-boilerplate
"
date: 2016-08-16 21:00:41
categories: 
- Loopback
tags:
- Vue
- Webpack
- Javascript
- Loopback
- Node.js
---



Loopback在國外被許多企業公司採用，是個強大的RESTAPI server，但是目前卻沒有與Vue.js結合的範本。

所以就花了點時間研究一下，拼拼湊湊出了，希望能夠幫到更多人。

 <!--more-->
 
# loopback-vue-boilerplate

A boilerplate for a Vue application using LoopBack

[Github repo](https://github.com/cslinmiso/loopback-vue-boilerplate.git)

### Get Started
- **Clone this repository or use npm**
```bash
$ git clone https://github.com/cslinmiso/loopback-vue-boilerplate.git
```
```bash
$ npm install loopback-vue-boilerplate
```

- **Install dependencies specified in package.json**
```bash
$ npm install
```

- **Start the server (default port is set to 3000)**
```bash
$ npm start
```

### Scripts
- **npm run build**: Bundles the application into `.build/`.

- **npm run dev**: Starts developer server, contains webpack hot module replacement.

- **npm run prod**: Starts production server, make sure you have already deployed the application.

- **npm run clean**: Removes the bundled files.

### Built-in example
A simple 'Hello World' Vue.js application contains an example by Evan You (yyx990803) is included in this boilerplate. 
You can find those files under `/client`.

Hot reloading is only applied in development mode. In production mode, the code base is pre-compiled and placed under `.build/static`.

### License

[MIT](https://github.com/cslinmiso/loopback-vue-boilerplate/blob/master/LICENSE)

### Copyright

Copyright (C) 2016 Trey Lin, released under the MIT License.


### Reference
[loopback-redux-react-boilerplate](https://github.com/tngan/loopback-redux-react-boilerplate)