# automated-egg-steam

![image](https://github.com/jarvarbin/automated-egg-steam/assets/93614373/fb5073ed-a3b5-4040-9a79-ad7010631ca8)

![image](https://github.com/jarvarbin/automated-egg-steam/assets/93614373/66dfcf58-fd2a-4e24-ad47-e63efc3e6244)


This script is designed to automate the clicking process in a Steam game known as the "Egg Game". In this game, the main objective is to click on an egg as rapidly as possible. Players attempt to achieve the highest click rate without causing system instability. The optimal click rate is crucial for maximizing the number of clicks in a given period, potentially unlocking various in-game skins. These skins have a current market value of approximately 25 cents each.




# Description of the Code for the Egg Game on Steam

This script implements a multi-threaded automatic clicking program using the `threading`, `pyautogui`, `loguru`, and `time` libraries in Python. Below is a detailed explanation of its functionality:

## Library Imports

```

import threading
import pyautogui
from loguru import logger
import time

```

The following libraries are imported:

threading: For creating and managing multiple threads.
pyautogui: For automating mouse actions.
loguru: For logging events and messages.
time: For handling time-related functions.
Function Definitions
Function click_thread

```
def click_thread(duration):
    end_time = time.time() + duration
    while time.time() < end_time:
        pyautogui.click()
```

This function continuously performs mouse clicks for a specified duration.

Function perform_clicks

```
def perform_clicks(total_duration: float, num_threads: int = 20) -> int:
    threads = []
    start_time = time.time()

    for _ in range(num_threads):
        thread = threading.Thread(target=click_thread, args=(total_duration,))
        threads.append(thread)
        thread.start()

    for thread in threads:
        thread.join()

    end_time = time.time()
    actual_duration = end_time - start_time
    return actual_duration
```

This function manages the creation and execution of multiple threads (num_threads) that run the click_thread function for a total duration (total_duration). It returns the actual execution duration.

Function main


```
def main():
    logger.info("Starting the multi-threaded automatic clicking program")
    
    desired_duration = 10  # You can increase the time here
    num_threads = 9

    actual_duration = perform_clicks(desired_duration, num_threads)
    
    # Count the clicks performed
    num_clicks = pyautogui.PAUSE * (desired_duration / pyautogui.PAUSE) * num_threads
    
    logger.info(f"Clicks performed in approximately {actual_duration:.2f} seconds")
    clicks_per_second = num_clicks / actual_duration
    logger.info(f"Estimated average speed: {clicks_per_second:.2f} clicks per second")
```

The main function initializes the logging, sets the desired duration and number of threads, and calculates the number of clicks performed along with the average clicking speed.

Execution

```
if __name__ == "__main__":
    pyautogui.PAUSE = 0.008  # Adjust this value to control the speed
    main()
```

The script execution starts here, setting the pause duration for pyautogui and calling the main function.
