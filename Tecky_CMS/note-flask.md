# A VERY Simple Demo : Express <-> Flask

## 0. Diagram

Network: Client <-> Express <-> Flask

Steps:

1)  Client call Express `demo` route : GET http://127.0.0.1:8080/demo

2)  Express call Flask `api` route with request param '1234' : GET http://127.0.0.1:5000/api/1234

3)  Flask response with request param '1234' : RESPONSE `{ "result": "1234" }`

4)  Express response with request param '1234' copy from Flask : RESPONSE `{ "result": "1234" }`

5)  Client received `{ "result": "1234" }`

## 1. Start Flask Server

## Open New Terminal

Installation:

```bash
pip install flask
```

Using flask default port 5000

Start Server:

```bash
cd flask
flask run
```

## 2. Start Express Server

## Open Another New Terminal

Installation:

```bash
yarn install
```

Using common port 8080

Start Server:

``` bash
cd express
yarn start
```