# AI Calorie Tracker

Turn any photo of food into instant, structured nutritional data — no manual logging,
no lookup tables, no guesswork. Just snap a picture and let a vision-language model do
the work of a nutritionist in seconds.

`ai_calorie_tracker.py` pairs a multimodal LLM with a tightly engineered prompt to
transform raw pixels into clean, machine-readable nutrition data — turning any photo
into a structured, queryable dataset with a single function call.

## How it works

1. **Capture** — load an image of a food item.
2. **Encode** — the image is converted to base64 for transport.
3. **Analyze** — the image and a structured prompt are sent to a vision-capable model.
4. **Structure** — the model returns clean, validated JSON with calorie and macro estimates, ready to plug into any app, dashboard, or database.

## Tools & Libraries used

| Tool / Library                                               | Purpose                                                                                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| [OpenAI Python SDK](https://github.com/openai/openai-python) | Client used to call the chat completions API                                                                              |
| [OpenRouter](https://openrouter.ai/)                         | API gateway used as the `base_url`, so you can route requests to `gpt-4o-mini` (or other models) through a single API key |
| [Pillow (PIL)](https://python-pillow.org/)                   | Opening and inspecting the input image                                                                                    |
| `base64` / `io` / `os` (standard library)                    | Encoding the image into a base64 string for the API request                                                               |
| [IPython](https://ipython.org/)                              | `display(Markdown(...))` for pretty-printed output — **notebook/Jupyter only**                                            |

## ⚠️ Colab-specific code

This script was auto-exported from Colab, so it still contains a couple of things that
**only work inside Google Colab** and need to be changed to run it locally or in CI:

```python
from google.colab import userdata
api = userdata.get('openaiapi')
```

This pulls the API key from Colab's secret manager. Locally, replace it with an
environment variable instead:

```python
import os
api = os.environ["OPENAI_API_KEY"]
```

Similarly, `IPython.display` and the hardcoded `/content/pizza_slice.png` path assume a
notebook environment — see **Setup** below for the local equivalents.

## Setup

1. **Clone the repo and install dependencies:**

   ```bash
   pip install openai pillow python-dotenv
   ```

   (`python-dotenv` is optional, but handy for loading your API key from a `.env` file.)

2. **Set your API key** as an environment variable:

   ```bash
   export OPENAI_API_KEY="your-openrouter-or-openai-key"
   ```

3. **Replace the Colab-only lines** as described above.

4. **Update the image path** to point at a local file, e.g.:

   ```python
   img_path = "./images/pizza_slice.png"
   ```

5. **Run it:**
   ```bash
   python ai_calorie_tracker.py
   ```

## Example output

One photo in, a fully structured nutrition profile out:

```json
{
  "food_name": "Pizza slice (pepperoni)",
  "serving_description": "1 slice (approx 107g)",
  "calories": 285.0,
  "fat_grams": 12.0,
  "protein_grams": 12.0,
  "confidence_level": "Medium"
}
```
