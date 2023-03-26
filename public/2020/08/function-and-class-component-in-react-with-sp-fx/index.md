# Function And Class Component In React With SPFx


Introduction
------------

  
In this article, we will learn the different types of components that we can create in React

  
We will also learn which should be used in which conditions.

React provides us two different types of components:

1.  Function component
2.  Class component

React documentation for different components can be found in the below [link](https://reactjs.org/docs/components-and-props.html#function-and-class-components).

Difference
----------

**Function component**

1.  In the syntax point of view, we do not write class for function component rather we create it as a function
2.  It only accepts props as an argument
3.  We can not use state with functional component i.e. setState() and hence functional component are also known as stateless components
4.  In the Functional component, we can not use Lifecycle Hooks.

**Class component**

1.  In syntax point of view, we write a class which inherits from React.component
2.  It accepts both props as well as state
3.  We can use state and hence it is also known as stateful components
4.  In class component, we can use Lifecycle Hooks like componentDidMount, the component will unmount

Benefits
--------

**Benefits functional component**

1.  As we cannot use lifecycle-hooks and state, it is easy to read and test a functional component
2.  We end up writing less code and hence it is easy to read and understand
3.  Performance boosting
4.  We can use State Hook for storing local data to the component

**Benefits class component**

1.  If we want to control the lifecycle of the component we should use class component
2.  We can store local data in the state which can be used in the component
3.  It keeps tracks in changes to the data in the state, and reacts if there is any change in it

How to use in SharePoint Framework (SPFx)
-----------------------------------------

For SPFx, let's create a new solution for SPFx using the below command (Assuming the development environment is set up for SPFx)
```
mkdir reactComponents
cd reactComponents
yo @microsoft/sharepoint
```
Select different values which are asked while creating the solution, as mentioned below:

![Function and class component in react with SPFx](https://f4n3x6c5.stackpathcdn.com/article/function-and-class-component-in-react-with-spfx/Images/Clipboard01.png)

Let's open the code in Visual studio code and create functional and class components in SPFx

**Note**

By default, the component which is created for the web part is a class component

How to create a functional component in SPFx
--------------------------------------------

Open the file present in the solution in the below path

src -> web parts -> understandComponent -> components

Now to create a functional component, we can add below code in UnderstandComponent.tsx:
```
export interface IUnderstandFunctionComponentProps {    
title: string;    
}    

export const UnderstandFunctionComponent: React.FunctionComponent<IUnderstandFunctionComponentProps> = (props: IUnderstandFunctionComponentProps) => {    
return (    
<h1>{props.title}</h1>    
);    
};    
```
As we can see in the above code, we are not creating a class, rather it is just a function in typescript which has a type as React.FunctionComponent

In this component, we are accepting the title as a parameter and displaying it with tag.

How to create a class component in SPFx
---------------------------------------

Now to create a class component, we can add below code in UnderstandComponent.tsx:
```
export interface IUnderstandStateComponentProps {    
title: string;    
}    
export interface IUnderstandStateComponentState {    
count: number;    
}    
class UnderstandStateComponent extends React.Component  
<IUnderstandStateComponentProps, IUnderstandStateComponentState> {    
constructor(props: IUnderstandStateComponentProps) {    
super(props);    
this.state = {    
count: 0    
};    
}    

public render(): React.ReactElement  
<IUnderstandStateComponentProps> {    

return (    

<div>  
<h1>{this.props.title}</h1>  
<div>{this.state.count}</div>  
<button onClick={() => this.setState({ count: this.state.count + 1 })}>Add</button>  
</div>    
);    
}    
}  
```
From the above code, we can understand that we have created a class named UnderstandStateComponent which is inheriting from React.Component

In this component, we are using the title as property and count as a state and this count would increase on click on the Add button. We can use constructor as this is not a function but a class.

How to use it and render the component
--------------------------------------

Using function or class component has the same syntax and is very easy to use we can update the render method in our component with the below code:
```
export default class UnderstandComponent extends React.Component  
<IUnderstandComponentProps, {}> {    

public render(): React.ReactElement  
<IUnderstandComponentProps> {    

return (    
<div className={styles.understandComponent}>  
<div className={styles.container}>  
<div className={styles.row}>  
<UnderstandFunctionComponent title="This is a function component"\></UnderstandFunctionComponent>  
<UnderstandStateComponent title="This is a class compoent"\></UnderstandStateComponent>  
</div>  
</div>  
</div>    

);    
}    
}  
```
As we can see from the above code, we are using both the component in a similar manner and we are passing the props value also in a similar manner hence in terms of component rendering both are similar.

**Outcome**

![Image](https://f4n3x6c5.stackpathcdn.com/article/function-and-class-component-in-react-with-spfx/Images/OutcomeunderstandcomponentSPFx.gif)

Conclusion
----------

Hence, we should use the function component when we want to use it as a presentation component, which does not store any data value but only displays as per the values passed in the property. If we want to store data, we should use class component, which helps us have all lifecycle hooks
