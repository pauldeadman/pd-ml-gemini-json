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

    {"id":"v1_ChY5YnM3YXZleUNycUNrZFVQMTRmb09BEhY5YnM3YXZleUNycUNrZFVQMTRmb09B","status":"completed","usage":{"total_tokens":1349,"total_input_tokens":228,"input_tokens_by_modality":[{"modality":"text","tokens":228}],"total_cached_tokens":0,"total_output_tokens":238,"total_tool_use_tokens":0,"total_thought_tokens":883},"created":"2026-06-24T11:13:57Z","updated":"2026-06-24T11:13:57Z","service_tier":"standard","steps":[{"signature":"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","type":"thought"},{"content":[{"text":"{\"recipe_name\":\"Delicious Chocolate Chip Cookies\",\"ingredients\":[{\"name\":\"all-purpose flour\",\"quantity\":\"2 1/4 cups\"},{\"name\":\"baking soda\",\"quantity\":\"1 teaspoon\"},{\"name\":\"salt\",\"quantity\":\"1 teaspoon\"},{\"name\":\"unsalted butter (softened)\",\"quantity\":\"1 cup\"},{\"name\":\"granulated sugar\",\"quantity\":\"3/4 cup\"},{\"name\":\"packed brown sugar\",\"quantity\":\"3/4 cup\"},{\"name\":\"vanilla extract\",\"quantity\":\"1 teaspoon\"},{\"name\":\"large eggs\",\"quantity\":\"2\"},{\"name\":\"semisweet chocolate chips\",\"quantity\":\"2 cups\"}],\"instructions\":[\"Preheat the oven to 375°F (190°C).\",\"In a small bowl, whisk together the flour, baking soda, and salt.\",\"In a large bowl, cream together the butter, granulated sugar, and brown sugar until light and fluffy.\",\"Beat in the vanilla and eggs, one at a time.\",\"Gradually beat in the dry ingredients until just combined.\",\"Stir in the chocolate chips.\",\"Drop by rounded tablespoons onto ungreased baking sheets and bake for 9 to 11 minutes.\"]}","type":"text"}],"type":"model_output"}],"object":"interaction","model":"gemini-3.5-flash"}(base)


