# Host AppRouter

This is the host app router. It is responsible for routing between the microfrontends inside of a single application context.

```tsx
import React from 'react';
import { useLocation, useNavigate } from 'react-router-dom';
import {
    createBrowserRouter,
    RouterProvider,
} from "react-router-dom";
// @ts-ignore
import { SmoothScroll } from 'mf2/ThemeProvider';
// @ts-ignore
import Navbar from 'mf2/Navbar';
// @ts-ignore
import CherrytopFramework from 'mf2/CherrytopFramework';
// @ts-ignore
import Stonetowerpizza from 'stonetowerpizza/App';
// @ts-ignore
import Openfitness from 'openfitness/App';
// @ts-ignore
import SmartiCamera from 'camera/App';
// @ts-ignore
import AiChat from 'aichat/App';
// @ts-ignore
import Breadcrumbs from 'mf2/Breadcrumbs';
// @ts-ignore
import AdminDashboard from 'mf2/AdminDashboard';
// import { useUtilityStore } from 'mf2/utilities/store/utilityStore';
import App from '../App';
import { 
    aichatNavbarSchema, 
    familyappsNavbarSchema, 
    openfitnessNavbarSchema
} from '../config/navbarSchema';


// **** PARENT MFE Navigation Router/Adapter ****
const NavigationAdapter = (
    { children, ...props }: 
    { children: JSX.Element, [key: string]: any }
) => {
    const location = useLocation();
    const navigate = useNavigate();
    const { utilityStore } = props?.stores;
    
    // router.go(): has to be a function referring to the navigate inside of its router context
    const router = { go: (path: string) => navigate(path)};
    
    // experimental -- this is to keep the breadcrumbs aligned with the parent and ...
    // ... child app routers. might pull out into its own hook
    const Crumbs = () => (
        <Breadcrumbs links={location.pathname.split("/")
            .filter((path: string) => path)
            .map((path: string) => ({ name: path, path: `/${path}` }))
        } />
    );

    return (
        <>
            <Navbar layout={props.navbarSchema({ navigate, utilityStore })} />
            {/* These "providers" have to be inside of the router */}
            {/* This is kind of like a "map-staate-to-props" for all the microfrontends */}
            <SmoothScroll>
                {React.cloneElement(children, { ...props, router, Crumbs })}
            </SmoothScroll>
        </>
    );
};

function AppRouter({ data, stores }: { data?: any, stores?: any }) {

    const appRoutes = [
        {
            path: "/",
            element: (<App appConfig={data} />),
            navbarSchema: familyappsNavbarSchema
        },
        {
            path: "/openfitness",
            element: (<Openfitness />),
            navbarSchema: openfitnessNavbarSchema,
            children: [
                {
                    path: "/openfitness/test",
                    element: (
                        <>
                            {/** 
                             * Funny story: For this Microfrontend architecture. 
                             * The child path just needs to be defined here. Then it can be redefined 
                             * in the child MFE routes.childRoutes with its actual definition.
                             **/}
                        </>
                    ),
                },
                {
                    path: "/openfitness/planner",
                    element: (<></>),
                },
            ]
        },
        {
            path: "/aichat",
            element: (<AiChat />),
            navbarSchema: aichatNavbarSchema
        },
        {
            path: "/cherrytopframework",
            element: (<CherrytopFramework />),
            navbarSchema: familyappsNavbarSchema
        },
        {
            path: "/stonetowerpizza",
            element: (<Stonetowerpizza />),
            navbarSchema: familyappsNavbarSchema
        },
        {
            path: "/camera",
            element: (<SmartiCamera />),
            navbarSchema: familyappsNavbarSchema
        },
        {
            path: "/admin",
            element: (<AdminDashboard />),
            navbarSchema: familyappsNavbarSchema
        },
    ].map((route) => ({
        id: route.path,
        ...route,
        element: (
            <NavigationAdapter navbarSchema={route?.navbarSchema || {}} stores={stores}>
                {route.element}
            </NavigationAdapter>
        )
    }));

    const appRouter = createBrowserRouter(appRoutes);

    return <RouterProvider router={appRouter} />;
};

export default AppRouter;
```
