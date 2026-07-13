# Extract as JSON Ingredients and Recipe using Gemini 3.5 Flash

## Sample test data

Please extract the recipe from the following text.
The user wants to make delicious chocolate chip cookies.
They need 2 and 1/4 cups of all-purpose flour, 1 teaspoon of baking soda,
1 teaspoon of salt, 1 cup of unsalted butter (softened), 3/4 cup of granulated sugar,
3/4 cup of packed brown sugar, 1 teaspoon of vanilla extract, and 2 large eggs.
For the best part, they will need 2 cups of semisweet chocolate chips.
First, preheat the oven to 375°F (190°C). Then, in a small bowl, whisk together the flour,
baking soda, and salt. In a large bowl, cream together the butter, granulated sugar, and brown sugar
until light and fluffy. Beat in the vanilla and eggs, one at a time. Gradually beat in the dry
ingredients until just combined. Finally, stir in the chocolate chips. 
Drop by rounded tablespoons
onto ungreased baking sheets and bake for 9 to 11 minutes.

## Request Gemini to extract data into JSON

create environment variable GEMINI_API_KEY with token before running

```
export GEMINI_API_KEY=AQ........
```

```
curl -X POST "https://generativelanguage.googleapis.com/v1beta/interactions" \
    -H "x-goog-api-key: $GEMINI_API_KEY" \
    -H 'Content-Type: application/json' \
    -d '{
      "model": "gemini-3.5-flash",
      "input": "Please extract the recipe from the following text.\nThe user wants to make delicious chocolate chip cookies.\nThey need 2 and 1/4 cups of all-purpose flour, 1 teaspoon of baking soda,\n1 teaspoon of salt, 1 cup of unsalted butter (softened), 3/4 cup of granulated sugar,\n3/4 cup of packed brown sugar, 1 teaspoon of vanilla extract, and 2 large eggs.\nFor the best part, they will need 2 cups of semisweet chocolate chips.\nFirst, preheat the oven to 375°F (190°C). Then, in a small bowl, whisk together the flour,\nbaking soda, and salt. In a large bowl, cream together the butter, granulated sugar, and brown sugar\nuntil light and fluffy. Beat in the vanilla and eggs, one at a time. Gradually beat in the dry\ningredients until just combined. Finally, stir in the chocolate chips. Drop by rounded tablespoons\nonto ungreased baking sheets and bake for 9 to 11 minutes.",
      "response_format": {
        "type": "text",
        "mime_type": "application/json",
        "schema": {
          "type": "object",
          "properties": {
            "recipe_name": {
              "type": "string",
              "description": "The name of the recipe."
            },
            "prep_time_minutes": {
                "type": "integer",
                "description": "Optional time in minutes to prepare the recipe."
            },
            "ingredients": {
              "type": "array",
              "items": {
                "type": "object",
                "properties": {
                  "name": { "type": "string", "description": "Name of the ingredient."},
                  "quantity": { "type": "string", "description": "Quantity of the ingredient, including units."}
                },
                "required": ["name", "quantity"]
              }
            },
            "instructions": {
              "type": "array",
              "items": { "type": "string" }
            }
          },
          "required": ["recipe_name", "ingredients", "instructions"]
        }
      }
    }'
```

## Output extracted from full output

```
{"content":[{"text":
"{
"recipe_name":"Delicious Chocolate Chip Cookies",
"ingredients": [ 
  {"name":"all-purpose flour","quantity":"2 1/4 cups"},
  {"name":"baking soda","quantity":"1 teaspoon"},
  {"name":"salt","quantity":"1 teaspoon"},
  {"name":"unsalted butter (softened)","quantity":"1 cup"},
  {"name":"granulated sugar","quantity":"3/4 cup"},
  {"name":"packed brown sugar","quantity":"3/4 cup"},
  {"name":"vanilla extract","quantity":"1 teaspoon"},
  {"name":"large eggs","quantity":"2"},
  {"name":"semisweet chocolate chips","quantity":"2 cups"}],
"instructions":[
  "Preheat the oven to 375°F (190°C).",
  "In a small bowl, whisk together the flour, baking soda, and salt.",
  "In a large bowl, cream together the butter, granulated sugar, and brown sugar until light and fluffy.",
  "Beat in the vanilla and eggs, one at a time.",
  "Gradually beat in the dry ingredients until just combined.",
  "Stir in the chocolate chips.",
  "Drop by rounded tablespoons onto ungreased baking sheets and bake for 9 to 11 minutes."]
}
,"type":"text"}],"type":"model_output"}
```

## Full Output

    {"id":"v1_ChY5YnM3YXZleUNycUNrZFVQMTRmb09BEhY5YnM3YXZleUNycUNrZFVQMTRmb09B","status":"completed","usage":{"total_tokens":1349,"total_input_tokens":228,"input_tokens_by_modality":[{"modality":"text","tokens":228}],"total_cached_tokens":0,"total_output_tokens":238,"total_tool_use_tokens":0,"total_thought_tokens":883},"created":"2026-06-24T11:13:57Z","updated":"2026-06-24T11:13:57Z","service_tier":"standard","steps":[{"signature":"EssaCsgaAQw51sfy4hmYIDYYzb2wRNIiXajpVgLSJ2mnk8zmHUaEKoEEtbMetgktph2RFMPtybS3ICkv+dK0qkuPPpg524hRTDqrIB2te+wx8NysK5a+ND/
    ......
    xNo1O8BrTmf/6usYVpVgtnVTvlRtPJXn7DF9Z+VkY8JH+qQoCWVL9DwRdlVUDb9Gpm1bwAZKESTtBuvjJ6f5YA+UsH6tkFWvwnfB+iiBtjS0AChtkH4UAaoM+BlH36VwK1/rcf6gMZ1KgQ2nCnkXR7RGA==","type":"thought"},{"content":[{"text":"{\"recipe_name\":\"Delicious Chocolate Chip Cookies\",\"ingredients\":[{\"name\":\"all-purpose flour\",\"quantity\":\"2 1/4 cups\"},{\"name\":\"baking soda\",\"quantity\":\"1 teaspoon\"},{\"name\":\"salt\",\"quantity\":\"1 teaspoon\"},{\"name\":\"unsalted butter (softened)\",\"quantity\":\"1 cup\"},{\"name\":\"granulated sugar\",\"quantity\":\"3/4 cup\"},{\"name\":\"packed brown sugar\",\"quantity\":\"3/4 cup\"},{\"name\":\"vanilla extract\",\"quantity\":\"1 teaspoon\"},{\"name\":\"large eggs\",\"quantity\":\"2\"},{\"name\":\"semisweet chocolate chips\",\"quantity\":\"2 cups\"}],\"instructions\":[\"Preheat the oven to 375°F (190°C).\",\"In a small bowl, whisk together the flour, baking soda, and salt.\",\"In a large bowl, cream together the butter, granulated sugar, and brown sugar until light and fluffy.\",\"Beat in the vanilla and eggs, one at a time.\",\"Gradually beat in the dry ingredients until just combined.\",\"Stir in the chocolate chips.\",\"Drop by rounded tablespoons onto ungreased baking sheets and bake for 9 to 11 minutes.\"]}","type":"text"}],"type":"model_output"}],"object":"interaction","model":"gemini-3.5-flash"}(base)


Note: Thought signature has been redacted.
