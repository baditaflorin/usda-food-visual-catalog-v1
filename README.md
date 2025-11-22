# usda-food-visual-catalog-v1

# NutriArt: USDA Visual Catalog

## Project Overview
NutriArt is a tool designed to generate high-quality, artistic visual representations of food items found in the USDA FoodData Central database. It utilizes Google's Gemini Imagen 3 model to create images based on nutritional profiles and commits them directly to a GitHub repository.

## Storage Structure & Integration

To ensure scalability and comply with GitHub's directory listing limits (1,000 files per directory), this repository utilizes a **partitioned directory structure**. 

**Important:** External programs or scripts intending to consume or link to these images must replicate this logic to locate files correctly.

### Partitioning Logic

Files are stored in subdirectories derived from their USDA FDC ID. The partition key is calculated by dividing the ID by **1000** and flooring the result.

**Formula:**
`partition = Math.floor(fdcId / 1000)`

### File Paths

- **Images:** `catalog/images/{partition}/{fdcId}.jpg`
- **Metadata:** `catalog/metadata/{partition}/{fdcId}.json`

### Examples

| USDA FDC ID | Partition Calculation | Resulting Path |
|-------------|----------------------|----------------|
| `2685576`   | `floor(2685576 / 1000) = 2685` | `catalog/images/2685/2685576.jpg` |
| `123`       | `floor(123 / 1000) = 0`        | `catalog/images/0/123.jpg` |
| `1005`      | `floor(1005 / 1000) = 1`       | `catalog/images/1/1005.jpg` |

### Code Snippet (JavaScript/TypeScript)

```typescript
/**
 * Resolves the GitHub path for a given USDA food item.
 * @param {number} fdcId - The unique USDA FoodData Central ID
 */
function getPaths(fdcId) {
  const partition = Math.floor(fdcId / 1000);
  return {
    image: `catalog/images/${partition}/${fdcId}.jpg`,
    metadata: `catalog/metadata/${partition}/${fdcId}.json`
  };
}
```
