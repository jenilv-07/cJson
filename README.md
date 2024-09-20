In C, handling JSON data can be done manually or using a JSON parsing library. Manually parsing JSON can be complex and error-prone, so it's often better to use a JSON library like `cJSON` to load and manipulate JSON data.

Here's a step-by-step guide to loading and parsing JSON data in C using the `cJSON` library.

### 1. Install `cJSON` library

First, you'll need to install the `cJSON` library on your system. You can download it from its GitHub repository or use package managers:

#### On Linux (Ubuntu/Debian):
```bash
sudo apt-get install libcjson-dev
```

#### On macOS:
```bash
brew install cjson
```

### 2. Code Example: Loading and Parsing JSON Data

Here’s an example C program that demonstrates how to load JSON data into your program using `cJSON`.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "cJSON.h"

// Function to load JSON data from a string
void load_json(const char* json_string) {
    // Parse the JSON string
    cJSON* json = cJSON_Parse(json_string);
    if (json == NULL) {
        printf("Error parsing JSON data.\n");
        return;
    }

    // Extract data from JSON
    cJSON* name = cJSON_GetObjectItem(json, "name");
    cJSON* age = cJSON_GetObjectItem(json, "age");
    cJSON* city = cJSON_GetObjectItem(json, "city");

    // Check if the fields are present and valid
    if (cJSON_IsString(name) && (name->valuestring != NULL)) {
        printf("Name: %s\n", name->valuestring);
    }

    if (cJSON_IsNumber(age)) {
        printf("Age: %d\n", age->valueint);
    }

    if (cJSON_IsString(city) && (city->valuestring != NULL)) {
        printf("City: %s\n", city->valuestring);
    }

    // Clean up memory allocated by cJSON_Parse
    cJSON_Delete(json);
}

int main() {
    // Define a JSON string (typically this could be read from a file or API)
    const char* json_data = "{\"name\":\"John\", \"age\":30, \"city\":\"New York\"}";

    // Load and parse the JSON data
    load_json(json_data);

    return 0;
}
```

### Explanation:

1. **Include `cJSON.h`**: This is the header file for the `cJSON` library that allows you to work with JSON data.
   
2. **JSON Data**: The JSON string `{"name":"John", "age":30, "city":"New York"}` is used as an example input.

3. **`cJSON_Parse()`**: This function parses the JSON string and returns a `cJSON` object that represents the JSON data.

4. **Extract JSON Fields**:
   - `cJSON_GetObjectItem()` is used to extract individual fields (e.g., `name`, `age`, `city`).
   - It is important to check if the fields exist and their types (e.g., `cJSON_IsString()`, `cJSON_IsNumber()`).

5. **Clean Up**: `cJSON_Delete()` is used to free the memory allocated for the parsed JSON object.

### Output:
```
Name: John
Age: 30
City: New York
```

### 3. Compile and Run the Program

To compile and link the `cJSON` library with your program, you can use the following command:

```bash
gcc -o json_loader json_loader.c -lcjson
```

Then, run the program:

```bash
./json_loader
```

### Loading JSON from a File

If you want to load JSON data from a file, you can read the file contents into a string and then parse it.

```c
#include <stdio.h>
#include <stdlib.h>
#include "cJSON.h"

// Function to read the contents of a file into a string
char* read_file(const char* filename) {
    FILE* file = fopen(filename, "r");
    if (file == NULL) {
        printf("Error opening file.\n");
        return NULL;
    }

    fseek(file, 0, SEEK_END);
    long file_size = ftell(file);
    rewind(file);

    char* buffer = (char*)malloc(file_size + 1);
    fread(buffer, 1, file_size, file);
    buffer[file_size] = '\0';

    fclose(file);
    return buffer;
}

// Function to load and parse JSON from a file
void load_json_from_file(const char* filename) {
    char* json_data = read_file(filename);
    if (json_data != NULL) {
        load_json(json_data);
        free(json_data);
    }
}

int main() {
    // Load and parse JSON from a file
    load_json_from_file("data.json");
    return 0;
}
```

You would need a `data.json` file with JSON content for this to work.

### Conclusion:

Using `cJSON` is a convenient way to load, parse, and manipulate JSON data in C programs. It handles the complexity of parsing and allows you to focus on using the data in your program.
