# Remote AppRouter

```tsx
import React from "react";
import { useEffect } from "react";
import {
    createBrowserRouter,
    RouterProvider,
} from "react-router-dom";
import { useNavigate } from "react-router-dom";
// mfe components / remote modules
// @ts-ignore
import { useConfirm } from 'mf2/ConfirmProvider';
// @ts-ignore
import { useUtilityStore } from 'mf2/utilities/store/utilityStore';
// @ts-ignore
import { SmoothScroll } from 'mf2/ThemeProvider';
// @ts-ignore
import Navbar from 'mf2/Navbar';
// @ts-ignore
import Planning from 'mf2/Planning';
// local modules
import AppWrapper from '../App/AppWrapper';
import { navbarSchema } from '../config/navbarSchema';


// <NavigationAdapter/> is very similar to the parent <NavigationAdapter/>
// ... that comes from the framework one level up. This one handles routes ...
// ... within this microfrontend. Provides routing within this microfrontend + routes.
const NavigationAdapter = (
    { children, ...props }: 
    { children: JSX.Element, [key: string]: any }
) => {
    const navigate = useNavigate();

    // router.go(): has to be a function referring to the navigate inside of its router context
    const router = { go: (path: string) => navigate(path) };

    return (
        <>
            {/* These "providers" have to be inside of the router */}
            {/* This is kind of like a "map-state-to-props" for all the microfrontends */}
            <SmoothScroll>
                {React.cloneElement(children, { ...props, router })}
            </SmoothScroll>
        </>
    );
};

// **Note:
// * Super weird way that is required in order to have child routes inside of a ...
// * ... child mfe. Child route must also be defined a certain way in the parent mfe hosting ...
// * ... the parent <AppRouter />.
function AppRouter(props: any) {
    const utilityStore = useUtilityStore();
    const confirm = useConfirm();

    console.logs("AppRouter.props: ", props);
    const appRoutes = [
        {
            path: "/openfitness",
            element: (
                <AppWrapper 
                    data={props.data} 
                    stores={{ utilityStore, confirm }} 
                />
            )
        },
        {
            path: "/test",
            element: (<Planning />)
        },
    ].map((route) => ({
        id: route.path,
        ...route,
        element: (
            <NavigationAdapter {...props}>
                {route.element}
            </NavigationAdapter>
        )
    }));

    const appRouter = createBrowserRouter(appRoutes);

    return <RouterProvider router={appRouter} />;
};

export default AppRouter;
```
