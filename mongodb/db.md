### mongoose

#### 链接数据库

~~~css
const mongoose=require('mongoose')
mongoose.connect('mongodb+srv://lewis:<passwd>@<name>.mongdb.net/test?retryWrites=true')//这是atlas 免费的数据库
mongoose.connect（）是回调函数
mongoose.connect('地址',{useNewUrlParser:true},()=>console.log('链接成功了'))
//错误信息
mongoose.connection.on('error',()=>console.error('error'))


~~~

#### 设计数据库的schema 相当于mysql中的table

~~~css 
用户模型 schema
const mongoose=require('mongoose')
const {schema,model}=mongoose;

const userSchema=new schema({
    __v:{type:Number,select:false}
    name:{type:String,required:true},
    age:{type:Number,default:0},
    passwd:{type:String,required:true,select:false},//敏感信息不回在数据中返回,
    avatar_url:{type:String},
    gender:{type:String,enum:['male','female'],default:'male',required:true},
    headline:{type:String},//一句话简介
    locations:{type:[{type:String}]},//代表字符串数组
    business:{type:String},
    employments:{type:[{company:{type:String},job:{type:String}}]},
    educations:{type:[{school:{type:String},major:{type:String},diploma:{type:Number,enum:[1,2,3,4,5]},entrance_year:{type:String}}]},
    following:{
        type:[{type:Schema.Types.ObjectId,ref:"User"}],
        select:false
    }  
})

//生成模型 数据库的操作都是使用这个模行
module.exports=model('User',userSchema) User是数据库中的collection名

个人资料的schema

字段过滤
默认隐藏部分字段

const {fields}=ctx.query;//以逗号分隔
~~~

#### 多对多 设计

~~~css
qq :810642693
~~~



#### 使用model

~~~css
User.find()
User.findById()
User.findOne()
//新建用户
await new User({.....}).save() //会直接保存到数据库
更新
User.findByIdAndUpdate(id,{更新的数据})
删除
User.findByIdAndRemove(id)
查询特定字段的信息
User.findById(cxt.params.id).select('+educations')
User.findById(cxt.params.id).select('+following').populate('following')
~~~

