# queries

Reusable query functions to be used with `@tanstack/react-query`. Opininated use of `axios` is recommended to set up the client with any credentials needed.

## Dependencies

`axios`
`@tanstack/react-query`

### Reference

[https://github.com/axios/axios](Axios)

[https://github.com/tanstack/react-query](Tanstack)

```tsx
// general app queries
const queries = ({
    // General Query to use any query with a passed queryPath
    query: (queryPath: string, payload?: any, method?: string) => ({
        queryKey: [queryPath],
        queryFn: async () => payload 
            ? (await (client as any)[method || "post"](queryPath, payload)).data
            : (await (client as any)[method || "get"](queryPath)).data
    }),
    graphQuery: (queryPath: string, payload?: any, method?: string) => ({
        queryKey: ["graphql", queryPath],
        queryFn: async () => payload 
            ? (await (graphClient as any)[method || "post"](queryPath, payload)).data
            : (await (graphClient as any)[method || "get"](queryPath)).data
    }),
});
```
