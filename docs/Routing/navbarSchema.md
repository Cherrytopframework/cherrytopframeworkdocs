# navbarSchema

## Dependencies

```tsx
// @ts-ignore
import NavMenu from 'mf2/NavMenu';

// ** navbarSchema -- This will determine the structure/layout of the navbar
// ** Note: it does not have to be a function. It just needs to return an object
// ** ... with the same structure.
// ... icon driven: https://mui.com/material-ui/material-icons/
// ... drawer | menu | home | cart | title
// ... items will display in the order specified in array
// ... drawer | menu | cart => also take an anchor prop and a content prop that will 
// ... display in the drawer and determine drawers position
// ... cart is linked to its own store that can be accessed anywhere in the app
// ... cart drawer will use the state in the cart store to render its view
// Openfitness app navbar schema
export const openfitnessNavbarSchema = ({ utilityStore, navigate }: any) => ({
    left: {
        items: [
            {
                key: "drawer",
                anchor: "left",
                content: (<>Drawer Content</>),
                onClick: () => utilityStore.setDrawer({ 
                    open: true, 
                    anchor: "left",
                    onOpen: () => {},
                    content: <></>
                }),
                // can override default props
                buttonProps: { tooltip: "LEFT DRAWR" }
            },
            {
                key: "home",
                onClick: () => navigate('/')
            }
        ]
    },
    middle: {
        items: [
            {
                key: "title",
                content: "Openfitness2 Navbar from Framework",
            },
        ]
    },
    right: {
        items: [
            {
                key: "drawer",
                anchor: "right",
                content: (<>Drawer Content</>),
                onClick: () => utilityStore.setDrawer({ 
                    open: true, 
                    anchor: "left",
                    onOpen: () => {},
                    content: (
                        <>Drawer Content LOCAL</>
                    )
                })
            },
            {
                key: "cart",
                anchor: "right",
                content: (<>Drawer Content</>),
                onClick: () => utilityStore.setDrawer({ 
                    open: true, 
                    anchor: "right",
                    onOpen: () => {},
                    content: <></>
                })
            },
            {
                key: "menu",
                anchor: "right",
                content: (
                    <NavMenu
                        items={[
                            // {
                            //     key: "profile",
                            //     onClick: () => navigate("/profile")
                            // },
                            // {
                            //     key: "alert",
                            //     onClick: () => {
                            //         console.log("Trying to create Alert: ", utilityStore)
                            //         utilityStore.createAlert("success", "Alert created")
                            //     }
                            // },
                            // ...etc. ...
                        ]}
                    />
                )
            },
        ]
    },
});

// FamilyApps Navbar Schema
export const familyappsNavbarSchema = ({ navigate, utilityStore }: any) => ({
    left: {
        items: [
            {
                key: "home",
                onClick: () => navigate('/')
            },
        ]
    },
    middle: {
        items: [
            {
                key: "title",
                content: "Cherrytopframework (formerly FamilyApps) mfv1 0.0.2",
            },
        ]
    },
    right: {
        items: [
            {
                key: "menu",
                anchor: "right",
                content: (
                    <NavMenu
                        items={[
                            {
                                key: "placeholder10",
                                onClick: () => {
                                    // to navigate into a mfe and into one of its child routes ...
                                    // ... need to route to the mfe using the parent AppRouter/navigate
                                    navigate("/openfitness")
                                    // ... then navigate to the route in the mfe using the child's mfe's navigate
                                    // ... need to do this by drilling the child navigate into the navbarSchema using ...
                                    // ... overrideNavbarSchema() function
                                    // if (props?.router) props.router.go("/planner");
                                }
                            },
                            {
                                key: "alert",
                                onClick: () => utilityStore.createAlert("success", "Alert created")
                            },
                            {
                                key: "erroralert",
                                onClick: () => utilityStore.createAlert("error", "Error Alert created")
                            },
                            // ...etc. ...
                        ]}
                    />
                )
            },
        ]
    },
})

// Aichat Navbar Schema
export const aichatNavbarSchema = ({ navigate }: any) => ({
    left: {
        items: [
            {
                key: "home",
                onClick: () => navigate('/')
            },
        ]
    },
    middle: {
        items: [
            {
                key: "title",
                content: "Aichat mfv1 0.0.2 navbar from framework",
            },
        ]
    },
    right: {
        items: [
            {
                key: "menu",
                anchor: "right",
                content: (<></>)
            },
        ]
    },
})
```