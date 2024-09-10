<h1>Parent/Child and Sibling/Sibling Communication</h1>  

We’ve seen how container components communicate with presentational components by passing their state through props, but how do presentational components communicate changes to the container?
One idea would be to update the props directly like this:
///
function Presentational(props) {
  const buttonClickHandler = () => {
    props.isActive = !props.isActive
  }
  // rest of code
}
///
But this would not be correct because components should never directly update their props. Recall that React functional components should be pure functions and updating prop values directly would violate that principle.
In order for a presentation (stateless) component to communicate changes to a container (stateful) component, the container component must define and provide a way for the presentational component to communicate with it using a change handler function passed as a prop.

For example:
///
  function Container() {
    const [isActive, setIsActive] = useState(false);                              
                                  
    return (
      <>
        <Presentational active={isActive} toggle={setIsActive}/>
        <OtherPresentational active={isActive}/>
      </>
      );                          
    }
                          
    function Presentational(props) {
    return (
      <h1>Engines are {props.active}</h1>
      <button onClick={() => props.toggle(!props.active)}>Engine Toggle</button>
    );
    }
                              
    function OtherPresentational(props) {
    // render...
    }
///
In the example above, Container maintains the isActive state and passes setIsActive to Presentational through the toggle prop. When Presentational needs to communicate a change to the active prop, it uses the function passed to it through the toggle prop.
Using this pattern also indirectly results in communication between sibling components (components with a common parent), as shown in the example above. When Presentational communicates a change through toggle, it causes a state update in Container, which provides the updated value for isActive to both Presentational and OtherPresentational through the active prop.
