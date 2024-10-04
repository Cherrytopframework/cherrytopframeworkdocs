# Providers

Global providers for Cherrytopframework Microfrontend application's.
This is the "batteries-included" version of the Providers component. It includes `AlertProvider`, `ConfirmProvider`, `PageTransitionWrapper`, `ThemeProvider`, `QueryWrapper`, `Drawer`, and `LocalizationProvider`.

`LocalizationProvider` enables date and time localization.

`AlertProvider` enables alerting to the user with hooks.

`ConfirmProvider` enables confirmation prompts to the user with hooks.

`PageTransitionWrapper` enables the page transition animation.

`ThemeProvider` configures the theme provided to the MFE -> this is a globally provided theme to any microfrontends using the `<Provider/>`.

`QueryWrapper` enables an initial query to be used when rendering the MFE.

`Drawer` enables a reusable drawer that can be used from anywhere in the app using hooks.

## Dependencies

`@mui/x-date-pickers`
`@mui/x-date-pickers/AdapterMoment`
`moment`

### Reference

(MUI)[https://mui.com/x/react-date-pickers/](https://mui.com/x/react-date-pickers/)

(Moment)[https://momentjs.com/](https://momentjs.com/)

```tsx title="mf2/Providers"
import React from 'react'
import { LocalizationProvider } from '@mui/x-date-pickers';
import { AdapterMoment } from '@mui/x-date-pickers/AdapterMoment';
import AlertProvider from './AlertProvider'
import { ConfirmProvider } from './Confirm'
import { PageTransitionWrapper, ThemeProvider } from '../../../utilities/theme'
import QueryWrapper from '../wrappers/QueryWrapper/QueryWrapper'
import Drawer from '../../Mui/Drawer/Drawer';


const Providers = (
    { children, path }: 
    { 
        children: (callback?: { data: any }) => React.ReactNode
        path?: (paths: any) => string | boolean,
    }
) => {
    const handlePath = (paths: any) => path ? path(paths) : "";
    return (
        <LocalizationProvider dateAdapter={AdapterMoment}>
            <ThemeProvider>
                <PageTransitionWrapper>
                    {((typeof(path) === "boolean") && !path)
                        ? children()
                        : (
                            <QueryWrapper path={handlePath as (paths: any) => string}>
                                {({ data }) => children(data)}
                            </QueryWrapper>
                        )
                    }
                    <AlertProvider />
                    <ConfirmProvider />
                    <Drawer />
                </PageTransitionWrapper>
            </ThemeProvider>
        </LocalizationProvider>
    )
}

export default Providers
export type Providers = ReturnType<typeof Providers>
```