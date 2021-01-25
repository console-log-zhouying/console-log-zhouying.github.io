---
title: react父组件调用子组件的方法
categories: react
tags: react 父组件调用子组件
---

- 父组件

  ```react
  import React, {Component} from 'react';
  
  export default class Parent extends Component {
  　　render() {
  　　　　return(
  　　　　　<div>
          	<Child onRef={this.onRef} />
  　　　　　</div>
  　　　　)
  　　}
  
  　　onRef = (ref) => {
  　　　　this.child = ref
  　　}
  }
  ```
<!-- more -->

- 子组件

  ```react
  class Child extends Component {
  
    constructor(props){
        super(props);
        this.state = {value: ''};
    }
      
    componentDidMount(){
        this.props.onRef(this)
    }
  
    render(){
      return(
          <div>
              我是子组件
              <input value={this.state.value} type="text"/>
          </div>
      )
    }
  }
  ```