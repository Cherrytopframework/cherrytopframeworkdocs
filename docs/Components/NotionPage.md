# NotionPage

This takes in a path and returns the NotionPage data. Passing in a function will provide the paths available to the framework. The paths available in the `path()` function are auto-generated, automatically added to the framework, and made available in the QueryWrapper path property anytime a new route is created in the backend.

## Installation

```bash
ctf add mf2/NotionPage
```

## Usage

```tsx language="tsx" title="mf2/NotionPage"
import { Box, Divider, Typography } from "@mui/material";
import QueryWrapper from "../wrappers/QueryWrapper/QueryWrapper";
import MarkdownWrapper from "../wrappers/MarkdownWrapper/MarkdownWrapper";


interface NotionPageProps {
    path: (paths: any) => string // to wherever the notion page data is
};

const NotionPage = (props: NotionPageProps) => (
    <QueryWrapper {...props}>
        {({ data }) => (
            <Box>
                <NotionDataWrapper results={data?.notionPageContent?.results} />
            </Box>
        )}
    </QueryWrapper>
);

const NotionDataWrapper = (props: any) => {
    const data = props?.results 
        ? props.results.map((block: any) => ({
            ...block,
            content: block[block.type]
        }))
        : [];

    const formatType = (type: string) => ({
        "paragraph": "body1",
        "heading_1": "h1",
        "heading_2": "h2",
        "heading_3": "h3",
        "heading_4": "h4",
        "heading_5": "h5",
        "heading_6": "h6",
        "bulleted_list_item": "li",
        "numbered_list_item": "li",
        "to_do": "li",
    }[type] || "body1");

    const buildMarkdown = (content: any) => {
        function convertToMarkdownCodeBlock(inputString: string) {
            // Escape special characters such as { and }
            const escapedString = inputString
                .replace(/&/g, "&amp;")
                // .replace(/</g, "&lt;")
                // .replace(/>/g, "&gt;")
                // .replace(/{/g, "&#123;")
                // .replace(/}/g, "&#125;")
                .replace(/`/g, "\\`");

            // Add code block syntax
            const markdownCodeBlock = `\`\`\`${content.language}\n${escapedString}\n\`\`\``;

            return markdownCodeBlock;
        };

        return convertToMarkdownCodeBlock(
            Array.isArray(content?.rich_text) 
                ? content.rich_text
                    .map(({ plain_text }: { plain_text: string }) => plain_text)
                    .join(".\n")
                : ""
        );
    };

    return !data 
        ? null 
        : data.map((block: any) => block?.content?.rich_text 
            && block.content.rich_text
                .map((
                    { plain_text, annotations }: 
                    { plain_text: string, annotations: any }
                ) => (block.type === "divider")
                    ? <Divider key={block.id} />
                    : (block.type === "code")
                        ? (
                            <Box sx={{ width: "80vw", overflowX: "auto" }} key={block.id}>
                                <MarkdownWrapper key={block.id}>
                                    {(buildMarkdown(block.content))}
                                </MarkdownWrapper>
                            </Box>
                        ) : (
                            <Typography
                                key={block.id}
                                variant={formatType(block.type) as "body1"}
                                sx={{
                                    color: annotations?.color || "inherit",
                                    fontWeight: annotations?.bold ? "bold" : "inherit",
                                    textDecoration: annotations?.underline ? "underline" : "inherit",
                                    fontStyle: annotations?.italic ? "italic" : "inherit",
                                }}
                            >
                                {plain_text}
                            </Typography>
                    ))
        );
};

export default NotionPage
```
