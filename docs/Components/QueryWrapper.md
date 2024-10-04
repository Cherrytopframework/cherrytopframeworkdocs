# QueryWrapper

```jsx title="mf2/QueryWrapper"
import { ReactNode, Suspense } from 'react';
import { useQuery, QueryClientProvider, QueryClient } from '@tanstack/react-query';
import { CircularProgress } from '@mui/material';

import { queries, paths } from '../../../../utilities/api';


const queryClient = new QueryClient();

const QueryWrapper2 = ({
    path,
    children,
    options,
    loadingContent,
    errorContent,
    ...props
} : {
    path: (paths: any) => string,
    children: (data: any) => ReactNode
    options?: {
        method: string
        payload?: any
        graphql: false | boolean
    }
    loadingContent?: ReactNode,
    errorContent?: (error: any) => ReactNode,
    [key: string]: any
}) => {
    const queryPath = path(paths);
    const wrapperQuery = useQuery(
        // queries.query(queryPath)
        !options?.graphql
            ? queries.query(queryPath, options?.payload, options?.method)
            : queries.graphQuery(queryPath, options?.payload, options?.method)
    );

    const handleSuccess = () => {
        if (props?.onData) props.onData(wrapperQuery.data);
        return children({ data: wrapperQuery.data });
    };
    
    return ({
        pending: (<></>),
        loading: (loadingContent ? loadingContent : <CircularProgress />),
        error: (
            errorContent 
                ? errorContent(wrapperQuery.error) 
                : <>Something went wrong. {JSON.stringify(wrapperQuery.error, null, 2)}</>
        ),
        success: (
            <Suspense fallback={<CircularProgress />}>
                {handleSuccess()}
            </Suspense>
        )
    }[wrapperQuery.status])
};

const QueryWrapper = ({ 
    children, 
    ...args 
}: { 
    children: (data: any) => ReactNode, 
    path: (paths: any) => string,
    options?: {
        method: string
        payload?: any
        graphql: boolean
    }
}) => {
    return (
        <QueryClientProvider client={queryClient}>
            {/* @ts-ignore */}
            <QueryWrapper2 {...args}>
                {children}
            </QueryWrapper2>
        </QueryClientProvider>
    )
}

export default QueryWrapper;
```

## Usage

```jsx
...
const QueryWrapper = React.lazy(() => "mf2/QueryWrapper");

...
<List>
    <Suspense fallback="Loading...">
        <QueryWrapper 
            path={paths.path} 
            errorContent={(error) => (
                <>
                    Something went wrong!
                    {console.error("handleError", error)}
                </>
            )}
        >
            {({ data }) => data.map((item: any) => (
                <ListItem key={item.id}>
                    <ListItemText primary={item.primary} />
                </ListItem>
            ))}
        </QueryWrapper>
    </Suspense> 
</List>
...