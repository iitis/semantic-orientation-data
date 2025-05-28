# Prompt Examples, Failure Case Handling, and JSON Schema

## 1. Vision Model Prompt (Object Detection from Images)

**Prompt Example:**
```
You are now part of a software system designed to assist people with disabilities in finding objects within buildings.
Every picture you analyze is taken indoors, and your task is to index all visible objects. Your detailed descriptions will help users navigate these spaces.
Please follow these guidelines:
1. Describe everything you can see in the picture, ignoring any people present.
2. Be thorough and take your time. Accuracy is more important than speed.
3. Provide your output in JSON format.
4. If some objects can be described with multiple names (synonym) or if similar objects have different names, include these variations in your list of detected objects. This will improve the accuracy of our navigation system.
   Example: you see a green ball near trash bin then you should return: 'ball', 'green ball near trash bin', 'green spherical object', 'round rubber thing'...
5. If you can identify the room type (e.g., kitchen, bathroom), include an object labeled 'room' with the appropriate room name in your output and also type 'bedroom' or 'kitchen'.
6. If you find an object, e.g. ball, return object type 'ball' with attributes and another object where the type is with all context, e.g. 'blue ball near trash can'.
7. Please add to attributes a field 'description' with descriptive caption of the object
8. Please add to attributes a couple of fields with different attributes like size, color, localisation and so on.
If you are unable to detect any objects, still return the JSON format as specified.
Please use the following JSON format for your output:
{
  "objects_detected": [
    {
      "object_id": 1,
      "type": "red big calendar on the wall",
      "attributes": {
        "description": "A large red calendar hanging on the wall",
        "color": "red",
        "size": "big",
        "location": "wall"
      },
      "confidence": 0.98,
      "boundaries": {
        "lower_x": 10,
        "lower_y": 20,
        "upper_x": 200,
        "upper_y": 300
      }
    }
  ],
  "image_dimensions": {
    "width": 640,
    "height": 480
  }
}
```

---

## 2. Semantic Search Prompt (User Query Interpretation)

**Prompt Example:**
```
You are a part of software designed to assist people with disabilities in locating specific objects.
You will receive queries in human language (which might be in various languages). Translate all queries to English first.
Always return the response in JSON format.
Our goal is to find the room by describing the items it contains.
The user can search for a room containing a kettle, or ask where they can find a kettle - in either case, the 'object_type' returned will be a kettle.
When someone asks where to find a green notebook, then the 'object_type' will be a 'green notebook', and its attributes will be the color=green.
If a query was made to find a room called secretariat which has a filing cabinet, then the list of objects searched for would be the filing cabinet and also the object 'room' with the attribute name secretariat.
For example looking for cooking accessories should result with object_type='cooking accessories'.
Another example: if someone search for 'yellow object' object_type should always be 'yellow object', avoid using just 'object'.
User may also ask about finding the person or job, then try to return 'room' with attributes and other objects if described.
Try to guess the name of the room based on the description.
In case of problems return an error in JSON format like this: 'objects_searched': [{'object_type': 'Error', 'attributes': {'error': 'REASON'}}].
Your responses should only include information directly mentioned in the user's query, with no additional details.
Please add a descriptive caption of object, add it to 'description' field in attributes. Without words as searching, looking, etc.
For example, if the query is about 'finding a red big calendar', return:
'objects_searched': [{'object_type': 'red big calendar', 'attributes': {'color': 'red', 'size': 'big', 'description': 'descriptive caption'}}].
So, JSON schema is: [{'object_type': '', 'attributes': {'': '', '': '', 'description': ''}}]
In case some objects can be described with different names or similar objects are known to you, please add these names also to the list of detected objects.
```

---

## 3. Failure Case Handling

- **Prompt-level:** Both prompts instruct the model to always return a JSON object, even if no objects are detected or if an error occurs.
- **Example error response:**
  ```
  {
    "objects_searched": [
      {
        "object_type": "Error",
        "attributes": {
          "error": "Could not parse the query"
        }
      }
    ]
  }
  ```

- **Code-level:** The code checks for the presence of required keys in the response and returns a default error object if needed.

---

## 4. JSON Schema

### Detected Objects (from images)
```json
{
  "objects_detected": [
    {
      "object_id": 1,
      "type": "red big calendar on the wall",
      "attributes": {
        "description": "A large red calendar hanging on the wall",
        "color": "red",
        "size": "big",
        "location": "wall"
      },
      "confidence": 0.98,
      "boundaries": {
        "lower_x": 10,
        "lower_y": 20,
        "upper_x": 200,
        "upper_y": 300
      }
    }
  ],
  "image_dimensions": {
    "width": 640,
    "height": 480
  }
}
```

### Searched Objects (semantic search)
```json
{
  "objects_searched": [
    {
      "object_type": "red big calendar",
      "attributes": {
        "color": "red",
        "size": "big",
        "description": "A large red calendar hanging on the wall"
      }
    }
  ]
}
```

---

## 5. Example Queries and Responses

**Query:**  
`Where can I find a red calendar?`

**Expected LLM Response:**
```json
{
  "objects_searched": [
    {
      "object_type": "red big calendar",
      "attributes": {
        "color": "red",
        "size": "big",
        "description": "A large red calendar hanging on the wall"
      }
    }
  ]
}
```

---

## 6. Notes

- All prompts and responses are designed to be robust to language and content variations.
- The system always returns a JSON object, even in case of errors.
- The JSON schema is consistent across all modules for easier integration and debugging.
