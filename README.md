# Discussion Grader for CS5740@VT

***Note:** This project is largely adapted from [Canvas Local Speed Grader](https://github.com/nowucca/aitools-canvas-api) and [Discussion Grader](https://github.com/nowucca/aitools-discussion-grader), both of which were developed by [Steven Atkinson](https://www.linkedin.com/in/satkinson/).*

## Setup

### Prerequisite

Make sure that **you**:
- have proper access to the course (you are either a teacher or a TA, and you can create a Canvas API key),
- have Python (3.12 or above) and uv installed, and
- have access to an LLM API service
  - For VT users, this has been taken care of (more on this later).

and for the **discussion assignment** you'd like to grade:
- **(required)** "Off" is selected for "Anonymous Discussion", and
- **(optional but recommended)** "Graded" is checked.

### Steps

1. Clone this repository:

    ```bash
    git clone https://github.com/yuhangzheng94/cs5740-aitools-grader.git
    ```

2. Initialize project environment for both `/aitools-canvas-api` and `/aitools-discussion-grader`:
    ```bash
    cd aitools-canvas-api && uv sync && cd ..
    cd aitools-discussion-grade && uv sync && cd ..
    ```

3. Configure Canvas connection.

    First, run
    ```bash
    cd aitools-canvas-api && cp canvas_config.json.example canvas_config.json
    ```

    Then, update values for variables in the `canvas_config.json` file. For VT users, the file would look something like this:

    ```json
    {
        "canvas_url": "https://canvas.vt.edu",
        "api_key": "your_canvas_api_developer_key"
    }
    ```

    **Warning**: Do **NOT** share your canvas API key with anyone!

4. Configure AI connection.

    **For VT Users:** To get a personal API key, follow instructions on https://www.docs.arc.vt.edu/ai/011_llm_api_arc_vt_edu.html#llm-api-arc-vt-edu.

    ```bash
    export OPENAI_API_KEY="your-openai-api-key-here"
    ```

    You will need to run this for each new terminal session. 

    **Warning:** Again, do **NOT** share your AI API key with anyone!

## Usage

Change the current directory to `/aitools-canvas-api`:

```bash
cd aitools-canvas-api
```

For dry run, nothing will be posted back to Canvas; everything stays in your local device.

### Dry Run (Single Student)

```bash
uv run python canvas_speedgrader.py --course-id $COURSE_ID --discussion-id $DISCUSSION_ID --grader ./uv_grader_wrapper.py --only-student $STUDENT_ID --output results.json
```

where `$COURSE_ID` is the ID of the course, `$DISCUSSION_ID` is the ID of the discussion topic, and `$STUDENT_ID` is the Login ID of the student.

### Dry Run (Entire Class)

The only different from dry running on a single student is that `--only-student` is not set:
```bash
uv run python canvas_speedgrader.py --course-id $COURSE_ID --discussion-id $DISCUSSION_ID --grader ./uv_grader_wrapper.py --output results.json
```

### View Grading Data

For all things related, go to `aitools-discussion-grader/discussions/discussion_{discussion_id}`, where `{discussion_id}` is the ID of the discussion topic.

For grading and comments specifically, go to `submissions` under the directory, where each JSON file represents one submission.
