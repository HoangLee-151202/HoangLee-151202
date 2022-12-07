---
sidebar_position: 1
---

# ReactJS

## 1. Giới thiệu

- React là một thư viện **JavaScript** để xây dựng giao diện người dùng
- React hỗ trợ việc xây dựng **Components** để có thể sử dụng lại được
- React tự tạo ra **virtual DOM** , sau đó host vào bộ nhớ giúp cải thiện hiệu suất

## 2. Khái niệm chính

### 2.1. Giới thiệu về JSX

**JSX (Javascript XML)**: Là một dạng ngôn ngữ cho phép viết HTML vào trong Javascript 

```jsx
const element = <h1>Hello, BloomGoo!</h1>

function element(){ 
  return <h1>Hello, BloomGoo!</h1>;
}
```

**[camelCase](https://www.google.com/search?q=camelCase&source=hp&ei=s-WJY72IGLza2roPrP6JiAI&iflsig=AJiK0e8AAAAAY4nzwyxuNaP1H5GbE_6pQfjdvOPkRsrh&ved=0ahUKEwj9rdKp7tr7AhU8rVYBHSx_AiEQ4dUDCAk&uact=5&oq=camelCase&gs_lcp=Cgdnd3Mtd2l6EAMyBQgAEIAEMgUIABCABDIFCAAQgAQyBQgAEIAEMgUIABCABDIFCAAQgAQyBQgAEIAEMgUIABCABDIFCAAQgAQyBQgAEIAEUABYAGCNA2gAcAB4AIABWYgBWZIBATGYAQCgAQKgAQE&sclient=gws-wiz)**: Vì JSX gần với JavaScript nên React DOM sử dụng quy ước này đặt tên thuộc tính thay vì tên thuộc tính HTML 

```
class trở thành className trong JSX
```



### 2.2. Rendering Elements


### 2.3. Components và Props
<h2>Components</h2>

**Components** cho phép chia giao diện người dùng thành các phần độc lập, có thể tái sử dụng<br/>

**Components** có 2 loại là ***Function Components*** và ***Class Components***<br/>
  
- **Function Components**
```jsx
function Welcome(props){ 
  return <h1>Welcome to {props.name}!</h1>;
}
```

- **Class Components**
```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Welcome to {this.props.name}!</h1>;
  }
}
``` 
<br/>

Cách sử dụng Components
```jsx
const element = <Welcome name="BloomGoo"/>
``` 

**[Chú ý](https://reactjs.org/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized)**: Chữ cái đầu tiên của Components phải bắt đầu bằng chữ in hoa

```jsx
function Hello(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Hello name="Lê Văn Thành" />
      <Hello name="Đỗ Hoài Nam" />
      <Hello name="Lê Tiến Đạt" />
    </div>
  );
}
``` 

Khi React nhìn thấy một phần tử do người dùng định nghĩa nó sẽ chuyển các cuộc tính JSX  và thành phần con thành dạng đối tượng, được gọi là ***props***<br/>

TIPS: Bạn nên đặt tên props theo quan điểm riêng của thành phần hơn là ngữ cảnh mà nó đang được sử dụng <br/>

<h2>Props</h2>

**Props** không bao giờ được sửa đổi các thành phần trong nó (**Read-Only**)

```jsx
!!Sai
function Welcome(props){ 
  props.name = "Lê Huy Hoàng";
  return <h1>Welcome to {props.name}!</h1>;
}
```

Nếu muốn thay đổi giá trị đầu vào phải **sao chép** dữ liệu hoặc **tạo state** cho dữ liệu

- **Sao chép**
```jsx
function Welcome(props){ 
  let _props = {...props};
  _props.name = "Lê Huy Hoàng";
  return <h1>Welcome to {_props.name}!</h1>;
}
```

- **Tạo State**
```jsx
import { useState } from "react";

function Welcome(props){ 
  const [name, setName] = useState(props.name);
  setName("Lê Huy Hoàng");
  return <h1>Welcome to {name}!</h1>;
}
```



### 2.4. State và Lifecycle
<h2>State</h2>

```jsx
  const [state, setState] = useState(value);
```
- **value**: Giá trị khởi tạo
- **state**: Giá trị
- **setState**: Hàm cập nhật lại giá trị

**Chú ý**: Không thể gán giá trị bằng state
```jsx
!!Sai
const [state, setState] = useState('value');
state = 'correct'

!!Đúng
const [state, setState] = useState('value');
setState('correct')
```
Cách sử dụng state hiệu quả
- Dùng với hàm .map

```jsx
!!Sai
const [arr,setArr] = useState([]);
let _arr = [];
list.map(ele => {
  _arr.push(ele.id)
});
setArr(_arr);

!!Đúng
const [arr, setArr] = useState(list.map(ele => ele.id));
```

- Dùng prevState 
```jsx
!!Sai
const [state,setState] = useState(false);
let _state = state;
_state = true;
setState(_state);

!!Đúng
const [state, setState] = useState(false);
setState(prevState => !prevState);
```

- Dùng prevState với Object
```jsx
const [state,setState] = useState({
  status: false,
  name: 'Door' 
});

setState(prevState => ({
  status: !prevState.status,
  ...prevState
}));
```
- Dùng prevState với Array
```jsx
const [arr, setArr] = useState(list.map(ele => ele.id));

setArr((prevState) => prevState.concat([id]));
```
  
TIPS: Chỉ sử dụng state khi cần
```jsx
!!Sai
function Field(props) {
const [fullName, setFullName] = useState(`${props.firstName} ${props.lastName}`);

return fullName;
}

!!Đúng
function Field(props) {
let fullName = `${props.firstName} ${props.lastName}`;

return fullName;
}
```

<h2>Lifecycle</h2>

**[Lifecycle](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)** là vòng đời của components. Gồm 3 trạng thái: <br/>
1. Mounting: `Khởi tạo state` > `Render` > `Update DOM and refs` > `useEffect`
2. Updating: `Props mới & setState & force­Update` > `Render` > `Update DOM and refs` > `useEffect & useDidMountEffect`
3. Unmounting: `Đóng components`

### 2.5. Handling Events
Các sự kiện phản ứng được đặt tên bằng cách sử dụng camelCase <br/>
Có hai cách xử lý sự kiện là ***không truyền tham số*** và ***truyền tham số***
- **Không truyền tham số**
```jsx
function handleClick(e) {
  console.log("Đã click")
}

return (
  <button onClick={handleClick}>Click<button>
);
```
- **Truyền tham số**
```jsx
function handleClick(value) {
  console.log(value)
}

let nameClick = 'handleClick'

return (
  <button onClick={() => handleClick(nameClick)}>Click<button>
);
```

### 2.6. Conditional Rendering

### 2.7. Lists and Keys

### 2.8. Forms

### 2.9. Lifting State Up

### 2.10. Composition vs Inheritance

### 2.11. Thinking In React

## 3. Hướng dẫn nâng cao

## 4. HOOKS

## 5. FAQ
