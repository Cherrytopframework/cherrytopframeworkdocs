# AlertProvider

Not normally used as stand alone component but it can be used as long as there is a utilityStore in the tree.

## Dependencies

`mui/material` `mf2/utilities/store`

## Reference

[mui/material](https://mui.com/material-ui/react-alert/)

[mf2/utilities/store](http://localhost:3000/docs/components/Providers)

## Props

```tsx title="mf2/AlertProvider"
import { Alert, Collapse, Snackbar } from "@mui/material";
import { useUtilityStore } from "../../../utilities/store";


const AlertProvider = ({ children }: { children?: any }) => {
    const { alert, setAlert } = useUtilityStore();
    return (
        <>
            {children && children}
            <Collapse in={alert.open}>
                <Snackbar 
                    open={alert.open} 
                    autoHideDuration={4000} 
                    onClose={() => setAlert({ ...alert, open: false })}
                >
                    <Alert
                        severity={alert.severity}
                        onClose={() => setAlert({ ...alert, open: false })}
                    >
                        {alert.message}
                    </Alert>
                </Snackbar>
            </Collapse>
        </>
    )
};

export default AlertProvider;
```

## Usage

```tsx

```
