<h1 align="center">IJI Documentation Revision 1.0</h1>

# Disclaimer
This document provides an overview and does not contain proprietary information, for the protection of intellectual property.

# Fundamental Overview
The IJI architecture comprises two main parts: BERRY and KIJI. BERRY is the logical component of the IJI architecture, responsible for decision-making, deduction, and model control. KIJI is the language component, producing coherent language output as instructed by BERRY. Once the model is initiated, it follows a provided objective and interacts in real-time according to that objective until the objective is achieved or the model is instructed to stop. Below is a basic demonstration of how to use the model. Note that `live_input` and `live_output` are theoretical modules used to illustrate how the model should be employed for simplicity.

```python
import model        # IJI model Python module
import live_input   # Module for capturing live input    (Theoretical)
import live_output  # Module for sending live responses  (Theoretical)
import threading    # Enables concurrent execution of input and output processes
iji = model.Start()

def input_listen():
    connection = live_input.data()
    for user, condition, message in connection.listen():
        iji.send(user, condition, message)

def output_listen():
    while True:
        get_response = iji.response()
        if get_response:
            live_output.send(get_response)

def start():
    input_thread = threading.Thread(target=input_listen)
    output_thread = threading.Thread(target=output_listen)
    input_thread.start()
    output_thread.start()

if __name__ == "__main__":
    start()
```
# Key Features and Components
## 1. KIJI Overview
### 1.1 Generation System
KIJI receives instructions deduced by BERRY and generates coherent and relevant text. Past training data, which includes a diverse range of input-output pairs, aids KIJI in making accurate predictions.
## 2. BERRY Overview
### 2.1 Live I/O
BERRY continuously receives live input data. Simultaneously, it evaluates whether a response should be generated based on the input data it receives.
### 2.2 Objective Following
BERRY is provided with a set of objectives as parameters, which are encoded into a format it can understand to initiate the IJI architecture. If no objectives are provided, it generates a list of them. Once an objective is set, BERRY aligns with those objectives until they are completed in order.
### 2.3 Sidetracking
BERRY also has a sidetracking parameter that must be set upon initialisation. This parameter defines the maximum number of times BERRY can generate output heavily influenced by live input before returning to the subject matter determined by the objectives.
