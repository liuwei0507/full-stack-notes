1、graphql与restful 的区别



2、搭建hello-graphql

​	mkdir hello-graphql 新建目录

1）搭建server端服务

​	-- mkdir server

​	--cd server 

​	--npm init -y

​	--npm install express --save. 安装express

​	--在server目录下新建 app.js并且在app.js中写搭建服务端代码	

```js
//加载express库
const express = require('express');
const app = express();

//监听4000端口，启动服务
app.listen(4000,() => {
    console.log("now listening for request")
})


```

​	-- 执行 node app 启动服务



备注：如果想要改端口自动重启，需要使用 nodemon app来启动服务，而此时需要安装nodemon

执行sudo npm install nodemon -g 安装nodemon



-- 安装graphql 并且与express绑定：

npm install graphql express-graphql

-- 引入graphql

```js
const graphqlHttp = require('express-graphql');

//定义路由
app.use('/graphql',graphqlHttp({

}))
```



访问 http://localhost:4000/graphql 报错没有schema

 -- 定义schema文件

```js
const graphql = require('graphql')

const {GraphQLObjectType, GraphQLString} = require('graphql')

const BookType = GraphQLObjectType({
    name:"Book",
    fields: () => ({
        id: {type: GraphQLString },
        name: {type: GraphQLString },
        genre: {type: GraphQLString }
    })
})
```



提取根type

```js
const RootQuery = new GraphQLObjectType({
    name: "RootQueryType",
    fields: {
        book: {
            type: BookType,
            args: {id: {type: GraphQLString }},
            resolve(parent,args) {
                //从哪里得到数据，比如从数据库或者其他来源
            }
        }
    }
})

module.exports = new GraphQLSchema({
    query: RootQuery
})
```



在app中引入 schema文件，并且设置 graphical 为true

```js
const schema =  require('./schema/schema')

const app = express();

//定义路由
app.use('/graphql',graphqlHttp({
    schema,
    graphiql: true
}))
```



安装lodash 模拟数据给resolve： npm install lodash

--- 引入lodash 

```js
const _ = require('lodash')
```

--- 模拟数据

```js
const books = [
    {"name": " 算法导论", genre: "计算机科学", id: "1"},
    {"name": " 人性的弱点", genre: "社交", id: "2"},
    {"name": " 明朝那些事", genre: "历史", id: "3"}
]
```

--- 查询数据

```js
resolve(parent,args) {
    //从哪里得到数据，比如从数据库或者其他来源
    console.log(args)
    //mongodb  mysql  postgresql
    return _.find(books, {id: args.id})

}
```



在浏览器 http://localhost:4000/graphql中调试

输入

```js
{
  book(id: "2") {
    name
    genre
    id
  }
}
```

输出

```js
{
  "data": {
    "book": {
      "name": " 人性的弱点",
      "genre": "社交",
      "id": "2"
    }
  }
}
```



使用 graphql-playground来调试graphql

苹果安装。brew cask install graphql-playground

 在endpoint中输入http://localhost:4000/graphql即可进入调试页面





设置graphql 的id 为GraphqlID, 这样字符串和数字类型的ID都可以查询

```js
const {
    GraphQLObjectType,
    GraphQLString,
    GraphQLSchema,
    GraphQLID
} = require('graphql')


fields: () => ({
        id: {type: GraphQLID },
        name: {type: GraphQLString },
        genre: {type: GraphQLString }
    })
 

 args: {id: {type: GraphQLID }},
```





如果想定义多个查询对象，

```js
const AuthorType = new GraphQLObjectType({
    name:"Author",
    fields: () => ({
        id: {type: GraphQLID },
        name: {type: GraphQLString },
        age: {type: GraphQLInt }
    })
})
```

然后在rootquery中查询相应的数据

```js
author: {
    type: AuthorType,
    args: {id: {type: GraphQLID}},
    resolve(parent, args) {
        return _.find(authors,{id: args.id})
    }
}
```

查询关联关系

例如：查询书的作者信息 （多对一，返回对象）

--- 在书的数据结构中有作者的ID字段维护书和作者的关系

```js
const books = [
    {"name": " 算法导论", genre: "计算机科学", id: "1",authorId: "1" },
    {"name": " 人性的弱点", genre: "社交", id: "2", authorId: "2"},
    {"name": " 明朝那些事", genre: "历史", id: "3",authorId: "3"}
]
```

---- 在书的查询对象中，返回作者的信息

```js
const BookType = new GraphQLObjectType({
    name:"Book",
    fields: () => ({
        id: {type: GraphQLID },
        name: {type: GraphQLString },
        genre: {type: GraphQLString },
        author: {
            type: AuthorType,
            resolve(parent,args) {
                console.log(parent.authorId)
                return _.find(authors,{id: parent.authorId})
            }
        }
    })
})
```

---- 查询语句

```js
{
	book(id: "2") {
    name
    genre
    id
    author {
      name
      id
      age
    }
  }
}
```

返回

```js
{
  "data": {
    "book": {
      "name": " 人性的弱点",
      "genre": "社交",
      "id": "2",
      "author": {
        "name": " liuwei",
        "id": "2",
        "age": 26
      }
    }
  }
}
```





例如：查询一个作者的所有书（多对一，返回数组）

在AuthorType中定义返回的books数据格式为数组格式

```js
const AuthorType = new GraphQLObjectType({
    name:"Author",
    fields: () => ({
        id: {type: GraphQLID },
        name: {type: GraphQLString },
        age: {type: GraphQLInt },
        books: {
            //返回数组
            type: new GraphQLList(BookType),
            resolve(parent, args) {
                //传入自己的ID
                return _.filter(books,{authorId: parent.id})
            }
        }
    })
})
```

此时，需要在导入语句中导入数组对象

```js
const {
    GraphQLObjectType,
    GraphQLString,
    GraphQLSchema,
    GraphQLID,
    GraphQLInt,
    GraphQLList
} = require('graphql')

```



---- 查询语句

```js
{
  author(id: "2") {
    name
    id
    age
    books {
      name
      id
      genre
    }
  }
}
```

返回数据

```js
{
  "data": {
    "author": {
      "name": " liuwei",
      "id": "2",
      "age": 26,
      "books": [
        {
          "name": " 人性的弱点",
          "id": "2",
          "genre": "社交"
        },
        {
          "name": " 人性的弱点1",
          "id": "2",
          "genre": "社交"
        }
      ]
    }
  }
}
```



查询所有（返回列表）

在RootQuery中添加查询的语法 --- 没有请求参数，返回的是List

```js
 //查询所有book
        books: {
            type: GraphQLList(BookType),
            resolve(parent, args) {
                return books
            }
        },
        //查询所有的authors
        authors: {
            type: GraphQLList(AuthorType),
            resolve(parent, args) {
                return authors
            }
        }
```



查询语句--- 查询所有书籍

```js
{
  books {
    name
    author {
      name
    }
  }
}
```

返回

```js
{
  "data": {
    "authors": [
      {
        "name": " 刘伟",
        "age": 27,
        "books": [
          {
            "name": " 算法导论"
          }
        ]
      },
      {
        "name": " liuwei",
        "age": 26,
        "books": [
          {
            "name": " 人性的弱点"
          },
          {
            "name": " 人性的弱点1"
          }
        ]
      },
      {
        "name": " weiliu",
        "age": 30,
        "books": [
          {
            "name": " 明朝那些事"
          },
          {
            "name": " 明朝那些事1"
          }
        ]
      }
    ]
  }
}
```

查询所有作者

```js
{
  authors {
    name
    age
    books {
      name
    }
  }
}
```

返回

```js
{
  "data": {
    "authors": [
      {
        "name": " 刘伟",
        "age": 27,
        "books": [
          {
            "name": " 算法导论"
          }
        ]
      },
      {
        "name": " liuwei",
        "age": 26,
        "books": [
          {
            "name": " 人性的弱点"
          },
          {
            "name": " 人性的弱点1"
          }
        ]
      },
      {
        "name": " weiliu",
        "age": 30,
        "books": [
          {
            "name": " 明朝那些事"
          },
          {
            "name": " 明朝那些事1"
          }
        ]
      }
    ]
  }
}
```



使用graphql 连接mongodb数据库（采用线上的数据库. mlab.com）

--- 注册mlab账号

或者使用本地安装mongodb(安装配置需要补上)



使用npm install mongoose 安装mongodb依赖

在app.js中写上代码，连接mongodb

```js
const mongoose = require('mongoose')

const app = express();

//连接到mongodb
mongoose.connect("mongodb://localhost/graphql")
mongoose.connection.once('open', () => {
    cons
```

新建mongodb对象对应的实体类 author.ja 和book.js

Book.js

```js
const mongoose = require('mongoose')
const Schema = mongoose.Schema

//默认id自动生成
const bookSchema = new Schema({
    name:String,
    genre:String,
    authorId:String
})

module.exports = mongoose.model('Book',bookSchema)
```

Author.js

```js
const mongoose = require('mongoose')
const Schema = mongoose.Schema

//默认id自动生成
const authorSchema = new Schema({
    name:String,
    age:Number
})

module.exports = mongoose.model('Author',authorSchema)
```

在schema.js文件中导入这两个实体类

```js
const Book = require('../models/book')
const Author = require('../models/author')
```

使用mutation实现新增数据的操作

```js
const Mutation = new GraphQLObjectType({
    name:"Mutation",
    fields: {
        addAuthor: {
            type: AuthorType,
            args: {
                name: {type: GraphQLString},
                age:{type:GraphQLInt}
            },
            resolve(parent,args) {
                let author = new Author({
                    name: args.name,
                    age: args.age
                })
                return author.save()
            }
        }
    }
})
```

并且在export中暴露此方法

```js
module.exports = new GraphQLSchema({
    query: RootQuery,
    mutation: Mutation
})
```

在graphql页面插入数据

```js
 mutation {
    addAuthor(name:"liuwei",age:17) {
      name
      age
    }
  }
```

返回

```js
{
  "data": {
    "addAuthor": {
      "name": "liuwei",
      "age": 17
    }
  }
}
```



新增book

```js
addBook: {
    type:BookType,
    args: {
        name: {type: GraphQLString},
        genre: {type: GraphQLString},
        authorId: {type: GraphQLString}
    },
    resolve(parent, args) {
        let book = new Book({
            name: args.name,
            genre: args.genre,
            authorId: args.authorId
        })
        return book.save()
    }
}
```



改造（使用mongoldb 代替静态数据之后，查询）

//根据ID查询数据

```js
//根据id查询author
author: {
    type: AuthorType,
    args: {id: {type: GraphQLID}},
    resolve(parent, args) {
        // return _.find(authors,{id: args.id})
        return Author.findById(args.id)
    }
}
```

//查询所有

```js
//查询所有的authors
authors: {
    type: GraphQLList(AuthorType),
    resolve(parent, args) {
        // return authors
        return Author.find({})
    }
}
```



对于mutation，插入的时候，如果没有写相应的属性的话，则值为空

```js
mutation {
  addAuthor(name: "suifeng") {
    name
    age
  }
}
```

返回

```js
{
  "data": {
    "addAuthor": {
      "name": "suifeng",
      "age": null
    }
  }
}
```

解决这种问题的做法是，在保存的时候使用非空限制

```js
addAuthor: {
    type: AuthorType,
    args: {
        name: {type: new GraphQLNonNull(GraphQLString)},
        age:{type: new GraphQLNonNull(GraphQLInt)}
    },
    resolve(parent,args) {
        let author = new Author({
            name: args.name,
            age: args.age
        })
        return author.save()
    }
}
```





=================

搭建客户端（使用react）

安装全局react

npm install -g create-react-app.  

搭建react脚手架

create-react-app  client

========================

创建组件BookList.js

```js
import React,{Component} from "react";

export default class BookList extends Component{
    render() {
        return (
            <div>
                <ul id="book-list">
                    <li>Book Name</li>
                </ul>
            </div>
        )
    }
}
```



安装Apollo环境连接graphql

在client目录下执行 

npm install apollo-boost react-apollo graphql-tag graphql --save

在App.js中导入Apollo相关的包，并使用Apollo连接graphql服务端

```js
import { ApolloClient } from 'apollo-client'
import { HttpLink } from 'apollo-link-http'
import { InMemoryCache } from 'apollo-cache-inmemory'
import { ApolloProvider } from 'react-apollo'

const client = new ApolloClient({
    uri:"http://localhost:4000/graphql",
    link: new HttpLink(),
    cache: new InMemoryCache()
})
```

同时，使用ApolloProvider来包裹，然后发送请求

(查看官方文档。react apollo graphql)



解决跨域问题（即在3000端口请求4000服务器）

--- 在服务器端安装cors   npm install cors

--- 在服务器端使用cors

```js
const cors = require('cors')
app.use(cors())
```



apollo连接远程服务器获取数据并展示

--- 在BookList.js中执行graphql

```js
const getBooksQuery = gql`
    {
        books {
            name
            id
        }
    }
`;
```

BookList组件中获取数据并展示

```js
class BookList extends Component{

    displayBooks = () => {
        const data = this.props.data
        if(data.loading) {
            return (<div>Loading books...</div>)
        } else {
            return (
                <ul id="book-list">
                {
                    data.books.map(book => {
                        return (
                            <li key={ book.id }>{ book.name }</li>
                        )
                    })
                }
                </ul>
            )
        }
    }

    render() {
        console.log(this.props)
        return (
            <div>
                { this.displayBooks() }
            </div>
        )
    }
}

export default graphql(getBooksQuery)(BookList)
```



新增book

新建addBook组件，组件中需要关联author进行展示

```js
import React,{Component} from "react";
import { graphql } from "react-apollo";
import gql from 'graphql-tag'

const getAuthorsQuery = gql`
    {
        authors {
            name
            id
        }
    }
`;

class AddBook extends Component{
    displayAuthors = () => {
        const data = this.props.data
        if(data.loading) {
            return (<option disabled>Loading authors...</option>)
        } else {
            return (
                data.authors.map(author => {
                    return (
                        <option key={ author.id } value={ author.id }>{ author.name }</option>
                    )
                })
            )
        }
    }

    render() {
        return (
            <form>
                <div className="field">
                    <label>Book name:</label>
                    <input type="text"/>
                </div>
                <div className="field">
                    <label>Genre:</label>
                    <input type="text"/>
                </div>
                <div className="field">
                    <label>Author :</label>
                    <select>
                        <option>Select Author</option>
                        { this.displayAuthors() }
                    </select>
                </div>
            </form>
        )
    }
}
export default graphql(getAuthorsQuery)(AddBook)
```



将操作graphql的查询语句抽取到一个文件中

新建query.js文件

```js
import gql from 'graphql-tag'

const getBooksQuery = gql`
    {
        books {
            name
            id
        }
    }
`;

const getAuthorsQuery = gql`
    {
        authors {
            name
            id
        }
    }
`;

export { getBooksQuery, getAuthorsQuery }
```

在相应的组件中导入graphql查询

```js
import { getBooksQuery } from "../queries/queries";
```



将新增和查询的grapgql 放入一个js文件中中之后，在Addbook.js中同时引用这两个graphql语句的时候，Apollo2.6 中有compose，可以直接使用

```js
import { graphql,compose } from "react-apollo";
```

但是在3.0 之后compose没有了，需要采取其他方式导入

```js
import { flowRight as compose } from 'lodash'
```

导入之后，在default export 处

```js
export default compose(
    graphql(getAuthorsQuery, { name:"getAuthorsQuery" }),
    graphql(addBookMutation, { name: "addBookMutation" })
) (AddBook)
```

如此处理之后，this.props.data会是undefined，因此需要指定具体获取的是查询

```js
displayAuthors = () => {
    const data = this.props.getAuthorsQuery
    if(data.loading) {
        return (<option disabled>Loading authors...</option>)
    } else {
        return (
            data.authors.map(author => {
                return (
                    <option key={ author.id } value={ author.id }>{ author.name }</option>
                )
            })
        )
    }
}
```

处理完成之后，在submit 点击事件中增加