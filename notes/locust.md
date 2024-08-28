# Locust :cricket:

Purpose

- Justify the time investment in addressing resource failure recovery (memory and CPU threshold exceedances)
- Explain what locust is and its role in organizations
- Capabilities as a Python-based, open-source load testing tool that allows for scaleable and distributed testing of web apps and APIs

Problem statement

- Current Issues: problems faced with memory and CPU usage in your Locust setup
- Explain how these issues can lead to system instability or downtime, affecting the reliability of load testing results

Objectives

- Understanding how Locust handles resource management and identifying areas for improvement

Methodology

- Explain the steps taken to understand Locust's inner workings
- Including reviewing documentation, analyzing the codebase, and using profiling tools to monitor resource usage
- Highlight specific areas of Locust that were examined, such as its handling of concurrent users, memory allocation, and CPU load distribution

Implementation plan

- Provide a high-level overview of the solutions you plan to implement
- E.g., optimizing memory usage, implementing CPU throttling, or using monitoring tools to trigger alerts when thresholds are exceeded
- Briefly discuss the technical details of the proposed solutions, including any changes to the Locust configuration or the introduction of new tools or libraries

Conclusion

- Recap the benefits of implementing resource failure recovery, emphasizing the positive impact on system performance and testing reliability
- Call to Action: Encourage feedback and invite collaboration from colleagues to refine and implement the proposed solutions

---

What is Locust?

- Open source performance/load testing tool for HTTP and other protocols
- Developer-friendly :arrow_right: lets you define your tests in regular Python code

What are its capabilities?

- Command-line interface as well as web-based user interface
- Allows you to import regular Python libraries
- Pluggable architecture making it infinitely expandable
- Tests are not limited by a graphical user interface or domain-specific language

Features

- Locust runs every user inside its own greenlet (a lightweight process/coroutine)
- enables you to write your tests like normal (blocking) Python code instead of having to use callbacks or some other mechanism
- Locust makes it easy to run load tests distributed over multiple machines
- It is event-based (using gevent), which makes it possible for a single process to handle many thousands concurrent users
- The low overhead of each Locust user makes it very suitable for testing highly concurrent workloads
- Locust has a user friendly web interface that shows the progress of your test in real-time
- You can even change the load while the test is running
- It can also be run without the UI, making it easy to use for CI/CD testing
- Locust primarily works with websites/services, but can be used to test almost any system or protocol (just write a client)

## Locust semantics

- Locustfile
- Environment
- User
- gevent
- greenlet
- Runner
- Events
- Attributes
- Concurrent users
- Memory allocation
- CPU load distribution

---

- **Locust test:** essentially just a Python program making requests to the system you want to test
- **Locustfile:** normal Python module, can import code from other files or packages
- For a file to be a valid locustfile it must contain at least one class inheriting from `User`
- We define a class for the users that we will be simulating
- When a test starts, locust will create an instance of this class for every user that it simulates, and each of these users will start running within their own green gevent thread
- For every running User, Locust creates a greenlet (a coroutine or “micro-thread”), that will call methods decorated with `@task`
- Code within a task is executed sequentially (it is just regular Python code)
- Tasks are picked at random, but you can give them different weighting (how likely a task will be picked)
- When a task has finished executing, the `User` will then sleep for its specified wait time (`wait_time` attribute/method), then it will pick a new task
- Only methods decorated with `@task` will be picked, so you can define your own internal helper methods any way you like

- It’s important to remember that the locustfile.py is just an ordinary Python module that is imported by Locust
- From this module you’re free to import other python code just as you normally would in any Python program
- The current working directory is automatically added to python’s `sys.path`, so any python file/module/packages that resides in the working directory can be imported using the python `import` statement
- For small tests, keeping all the test code in a single `locustfile.py` should work fine, but for larger test suites, you’ll probably want to split the code into multiple files and directories
- How you structure the test source code is of course entirely up to you, but we recommend that you follow Python best practices
- Here’s an example file structure of an imaginary Locust project:

  ```text
  project_root/
  |-- common/
  |    |-- __init__.py
  |    |-- auth.py
  |    |__ config.py
  |-- locustfile.py
  |__ requirements.txt
  ```

- A project with multiple locustfiles could also keep them in a separate subdirectory:

  ```text
    project_root/
    |-- common/
    |    |-- __init__.py
    |    |-- auth.py
    |    |__ config.py
    |-- my_locustfiles/
    |    |-- api.py
    |    |__ website.py
    |__ requirements.txt
    ```

- With any of the above project structure, your locustfile can import common libraries using:

  ```python
  import common.auth
  ```

## Components and dependencies

### User

- A user class represents one type of user/scenario for your system
- When you do a test run you specify the number of concurrent users you want to simulate and Locust will create an instance per user
- You can add any attributes you like to these classes/instances, but there are some that have special meaning to Locust:
  - A User's `wait_time` method makes it easy to introduce delays after each task execution
  - If no wait_time is specified, the next task will be executed as soon as one finishes
  - Wait time is applied after task execution, so if you have a high spawn rate/ramp up you may end up exceeding your target during ramp-up
  - Wait times apply to tasks as a whole
  - A User class can have tasks declared as methods under it using the `@task` decorator, but one can also specify tasks using the `tasks` attribute
  - A User's `environment` attribute is a reference to the environment in which the user is running
  - Use this to interact with the environment, or the `runner` which it contains
  - Users can declare an `on_start` method and/or `on_stop` method
  - A User will call its `on_start` method when it starts running, and its `on_stop` method when it stops running

### Tasks

- When a load test is started, an instance of a `User` class will be created for each simulated user and they will start running within their own greenlet
- When these users run they pick tasks that they execute, sleep for awhile, and then pick a new task and so on
- The easiest way to add a task for a User is by using the `task` decorator
- Another way to define the tasks of a User is by setting the `tasks` attribute
- 

### Events

- If you want to run some setup code as part of your test, it is often enough to put it at the module level of your locustfile, but sometimes you need to do things at particular times in the run
- For this need, Locust provides event hooks
- If you need to run some code at the start or stop of a load test, you should use the `test_start` and `test_stop` events

  ```python
  from locust import events

  @events.test_start.add_listener
  def on_test_start(environment, **kwargs):
      print("A new test is starting")
  
  @events.test_stop.add_listener
  def on_test_stop(environment, **kwargs):
      print("A new test is ending")
  ```

- The `init` event is triggered at the beginning of each Locust process
- This is especially useful in distributed mode where each worker process (not each user) needs a chance to do some initialization
- For example, let’s say you have some global state that all users spawned from this process will need:

  ```python
  from locust import events
  from locust.runners import MasterRunner
  
  @events.init.add_listener
  def on_locust_init(environment, **kwargs):
      if isinstance(environment.runner, MasterRunner):
          print("I'm on master node")
      else:
          print("I'm on a worker or standalone node")
  ```

- Every user instance has its own connection pool, similar to how real users (browsers) would interact with a web server
- If you instead want to share connections, you can use a single pool manager
- To do this, set `pool_manager` class attribute to an instance of `urllib3.PoolManager`:

  ```python
  from locust import HttpUser
  from urllib3 import PoolManager
  
  class MyUser(HttpUser):
      # All instances of this class will be limited to 10 concurrent connections at most.
      pool_manager = PoolManager(maxsize=10, block=True)
  ```

- For more configuration options, refer to the urllib3 documentation

### Environment
### Runner
### gevent
### greenlet

## Event hooks

- Locust comes with a number of event hooks that can be used to extend Locust in different ways
- When running locust in distributed mode, it may be useful to do some setup on worker nodes before running your tests
- You can check to ensure you aren’t running on the master node by checking the type of the node’s `runner`:

  ```python
  from locust import events
  from locust.runners import MasterRunner
  
  @events.test_start.add_listener
  def on_test_start(environment, **kwargs):
      if not isinstance(environment.runner, MasterRunner):
          print("Beginning test setup")
      else:
          print("Started test from Master node")
  
  @events.test_stop.add_listener
  def on_test_stop(environment, **kwargs):
      if not isinstance(environment.runner, MasterRunner):
          print("Cleaning up test data")
      else:
          print("Stopped test from Master node")
  ```

### Run a background greenlet

- Because a locust file is “just code”, there is nothing preventing you from spawning your own greenlet to run in parallel with your actual load/Users
- For example, you can monitor the fail ratio of your test and stop the run if it goes above some threshold:

  ```python
  import gevent
  from locust import events
  from locust.runners import STATE_STOPPING, STATE_STOPPED, STATE_CLEANUP, MasterRunner, LocalRunner
  
  def checker(environment):
      while not environment.runner.state in [STATE_STOPPING, STATE_STOPPED, STATE_CLEANUP]:
          time.sleep(1)
          if environment.runner.stats.total.fail_ratio > 0.2:
              print(f"fail ratio was {environment.runner.stats.total.fail_ratio}, quitting")
              environment.runner.quit()
              return
  
  
  @events.init.add_listener
  def on_locust_init(environment, **_kwargs):
      # dont run this on workers, we only care about the aggregated numbers
      if isinstance(environment.runner, MasterRunner) or isinstance(environment.runner, LocalRunner):
          gevent.spawn(checker, environment)
  ```

### Test data management

- There are a number of ways to get test data into your tests (after all, your test is just a Python program and it can do whatever Python can)
- Locust’s events give you fine-grained control over when to fetch/release test data
- Detailed example:

  ```python
  # This example shows the various ways to run things before/outside of the normal task execution flow,
  # which is very useful for fetching test data.
  #
  # 1. Locustfile parse time
  # 2. Locust start (init)
  # 3. Test start
  # 4. User start
  # 5. Inside a task
  # M1. CPU & memory usage
  # M2. master sent heartbeat to worker 1-N
  # M3. worker 1-N received heartbeat from master
  # (M* are repeated as long as locust is running)
  # ...
  # 6. Test run stopping
  # 7. User stop
  # 8. Test run stop
  # (3-8 are repeated if you restart the test in the UI)
  # 9. Locust quitting
  # 10. Locust quit
  #
  # try it out by running:
  #  locust -f test_data_management.py --headless -u 2 -t 5 --processes 2
  from __future__ import annotations
  
  from locust import HttpUser, events, task
  from locust.env import Environment
  from locust.runners import MasterRunner
  from locust.user.wait_time import constant
  
  import datetime
  from typing import Any
  
  import requests
  
  
  def timestring() -> str:
      now = datetime.datetime.now()
      return datetime.datetime.strftime(now, "%m:%S.%f")[:-5]
  
  
  print("1. Parsing locustfile, happens before anything else")
  
  # If you want to get something over HTTP at this time you can use `requests` directly:
  global_test_data = requests.post(
      "https://postman-echo.com/post",
      data="global_test_data_" + timestring(),
  ).json()["data"]
  
  test_run_specific_data = None
  
  
  @events.init.add_listener
  def init(environment: Environment, **_kwargs: Any) -> None:
      print("2. Initializing locust, happens after parsing the locustfile but before test start")
  
  
  @events.quitting.add_listener
  def quitting(environment: Environment, **_kwargs: Any) -> None:
      print("9. locust is about to shut down")
  
  
  @events.test_start.add_listener
  def test_start(environment: Environment, **_kwargs) -> None:
      # happens only once in headless runs, but can happen multiple times in web ui-runs
      global test_run_specific_data
      print("3. Starting test run")
      # in a distributed run, the master does not typically need any test data
      if not isinstance(environment.runner, MasterRunner):
          test_run_specific_data = requests.post(
              "https://postman-echo.com/post",
              data="test-run-specific_" + timestring(),
          ).json()["data"]
  
  
  @events.heartbeat_sent.add_listener
  def heartbeat_sent(client_id: str, timestamp: float) -> None:
      print(f"M2. master sent heartbeat to worker {client_id} at {datetime.datetime.fromtimestamp(timestamp)}")
  
  
  @events.heartbeat_received.add_listener
  def heartbeat_received(client_id: str, timestamp: float) -> None:
      print(f"M3. worker {client_id} received heartbeat from master at {datetime.datetime.fromtimestamp(timestamp)}")
  
  
  @events.usage_monitor.add_listener
  def usage_monitor(environment: Environment, cpu_usage: float, memory_usage: int) -> None:
      # convert from bytes to Mebibytes
      memory_usage = memory_usage / 1024 / 1024
      print(f"M1. {environment.runner.__class__.__name__}: cpu={cpu_usage}%, memory={memory_usage}M")
  
  
  @events.quit.add_listener
  def quit(exit_code: int, **kwargs: Any) -> None:
      print(f"10. Locust has shut down with code {exit_code}")
  
  
  @events.test_stopping.add_listener
  def test_stopping(environment: Environment, **_kwargs: Any) -> None:
      print("6. stopping test run")
  
  
  @events.test_stop.add_listener
  def test_stop(environment: Environment, **_kwargs: Any) -> None:
      print("8. test run stopped")
  
  
  class MyUser(HttpUser):
      host = "https://postman-echo.com"
      wait_time = constant(180)  # be nice to postman-echo
      first_start = True
  
      def on_start(self) -> None:
          if MyUser.first_start:
              MyUser.first_start = False
              # This is useful for similar things as to test_start, but happens in the context of a User
              # In the case of a distributed run, this would be run once per worker.
              # It will not be re-run on repeated runs (unless you clear the first_start flag)
              print("X. Here's where you would put things you want to run the first time a User is started")
  
          print("4. A user was started")
          # This is a good place to fetch user-specific test data. It is executed once per User
          # If you do not want the request logged, you can replace self.client.<method> with requests.<method>
          self.user_specific_testdata = self.client.post(
              "https://postman-echo.com/post",
              data="user-specific_" + timestring(),
          ).json()["data"]
  
      @task
      def t(self) -> None:
          self.client.get(f"/get?{global_test_data}")
          self.client.get(f"/get?{test_run_specific_data}")
          self.client.get(f"/get?{self.user_specific_testdata}")
  
          print("5. Getting task-run-specific testdata")
          # If every iteration is meant to use new test data this is the most common way to do it
          task_run_specific_testdata = self.client.post(
              "https://postman-echo.com/post",
              data="task_run_specific_testdata_" + timestring(),
          ).json()["data"]
          self.client.get(f"/get?{task_run_specific_testdata}")
  
      def on_stop(self) -> None:
          # this is a good place to clean up/release any user-specific test data
          print("7. a user was stopped")
  ```

- Locust provides support for distributing test data from master to workers while maintaining test data order using distributors ([example](https://github.com/SvenskaSpel/locust-plugins/blob/master/examples/distributor_ex.py), [source](https://github.com/SvenskaSpel/locust-plugins/blob/master/locust_plugins/distributor.py))
- Locust provides support for getting test data into your tests using readers ([CSV example](https://github.com/SvenskaSpel/locust-plugins/blob/master/examples/csvreader_ex.py), [CSV example source](https://github.com/SvenskaSpel/locust-plugins/blob/master/locust_plugins/csvreader.py), [MongoDB example](https://github.com/SvenskaSpel/locust-plugins/blob/master/examples/mongoreader_ex.py), [MongoDB example source](https://github.com/SvenskaSpel/locust-plugins/blob/master/locust_plugins/mongoreader.py))

## Distributed load generation

- A single process running Locust can simulate a reasonably high throughput
- For a simple test plan and small payloads it can make more than a thousand requests per second
- But if your test plan is complex or you want to run even more load, you’ll need to scale out to multiple processes, maybe even multiple machines
- Fortunately, Locust supports distributed runs out of the box

- To do this, you start one instance of Locust with the `--master` flag and one or more using the `--worker` flag
- The master instance runs Locust’s web interface, and tells the workers when to spawn/stop Users
- The worker instances run your Users and send statistics back to the master
- The master instance doesn’t run any Users itself
- To simplify startup, you can use the `--processes` flag
- It will launch a master process and the specified number of worker processes
- It can also be used in combination with `--worker`, then it will only launch workers
- Because Python cannot fully utilize more than one core per process (see GIL), you need to run one worker instance per processor core in order to have access to all your computing power
- There is almost no limit to how many Users you can run per worker
- Locust/gevent can run thousands or even tens of thousands of Users per process just fine, as long as their total request rate (RPS) is not too high
- If Locust is getting close to running out of CPU resources, it will log a warning

### Single machine

- It is really simple to launch a master and 4 worker processes:

  ```shell
  locust --processes 4
  ```
  
- You can even auto-detect the number of logical cores in your machine and launch one worker for each of them:

  ```shell
  locust --processes -1
  ```

### Multiple machines

- Start locust in master mode on one machine:

  ```shell
  locust -f my_locustfile.py --master
  ```

- And then on each worker machine:

  ```shell
  locust -f - --worker --master-host <your master> --processes 4
  ```

- The `-f`-argument tells Locust to get the locustfile from master instead of from its local filesystem

### Communicating across nodes

- When running Locust in distributed mode, you may want to communicate between master and worker nodes in order to coordinate the test
- This can be easily accomplished with custom messages using the built in messaging hooks:

  ```python
  from locust import events
  from locust.runners import MasterRunner, WorkerRunner
  
  # Fired when the worker receives a message of type 'test_users'
  def setup_test_users(environment, msg, **kwargs):
      for user in msg.data:
          print(f"User {user['name']} received")
      environment.runner.send_message('acknowledge_users', f"Thanks for the {len(msg.data)} users!")
  
  # Fired when the master receives a message of type 'acknowledge_users'
  def on_acknowledge(msg, **kwargs):
      print(msg.data)
  
  @events.init.add_listener
  def on_locust_init(environment, **_kwargs):
      if not isinstance(environment.runner, MasterRunner):
          environment.runner.register_message('test_users', setup_test_users)
      if not isinstance(environment.runner, WorkerRunner):
          environment.runner.register_message('acknowledge_users', on_acknowledge)
  
  @events.test_start.add_listener
  def on_test_start(environment, **_kwargs):
      if not isinstance(environment.runner, WorkerRunner):
          users = [
              {"name": "User1"},
              {"name": "User2"},
              {"name": "User3"},
          ]
          environment.runner.send_message('test_users', users)
  ```

- Note that when running locally (i.e. non-distributed), this functionality will be preserved; the messages will simply be handled by the runner that sends them
- Using the default options while registering a message handler will run the listener function in a blocking way, resulting in the heartbeat and other messages being delayed for the amount of the execution
- If you think that your message handler will need to run for more than a second then you can register it as concurrent
- Locust will then make it run in its own greenlet. Note that these greenlets will never be join():ed:

  ```python
  from locust import HttpUser, between, events, task
  from locust.runners import MasterRunner, WorkerRunner
  
  import gevent
  
  usernames = []
  
  
  def setup_test_users(environment, msg, **kwargs):
      # Fired when the worker receives a message of type 'test_users'
      usernames.extend(map(lambda u: u["name"], msg.data))
      environment.runner.send_message("acknowledge_users", f"Thanks for the {len(msg.data)} users!")
      environment.runner.send_message("concurrent_message", "Message to concurrent handler")
  
  
  def on_acknowledge(msg, **kwargs):
      # Fired when the master receives a message of type 'acknowledge_users'
      print(msg.data)
  
  
  def on_concurrent_message(msg, **kwargs):
      print(f"concurrent_message received with data: '{msg.data}'")
      gevent.sleep(10)  # if this handler was run with concurrent=False it would halt the message handling loop in locust
      print("finished processing concurrent_message")
  
  
  @events.init.add_listener
  def on_locust_init(environment, **_kwargs):
      if not isinstance(environment.runner, MasterRunner):
          environment.runner.register_message("test_users", setup_test_users)
      if not isinstance(environment.runner, WorkerRunner):
          environment.runner.register_message("acknowledge_users", on_acknowledge)
          environment.runner.register_message("concurrent_message", on_concurrent_message, concurrent=True)
  
  
  @events.test_start.add_listener
  def on_test_start(environment, **_kwargs):
      # When the test is started, evenly divides list between
      # worker nodes to ensure unique data across threads
      if not isinstance(environment.runner, WorkerRunner):
          users = []
          for i in range(environment.runner.target_user_count):
              users.append({"name": f"User{i}"})
  
          worker_count = environment.runner.worker_count
          chunk_size = int(len(users) / worker_count)
  
          for i, worker in enumerate(environment.runner.clients):
              start_index = i * chunk_size
  
              if i + 1 < worker_count:
                  end_index = start_index + chunk_size
              else:
                  end_index = len(users)
  
              data = users[start_index:end_index]
              environment.runner.send_message("test_users", data, worker)
  
  
  class WebsiteUser(HttpUser):
      host = "http://127.0.0.1:8089"
      wait_time = between(2, 5)
  
      def __init__(self, parent):
          self.username = usernames.pop()
          super().__init__(parent)
  
      @task
      def task(self):
          print(self.username)
  ```

## Resource management and utilization

- 

## Credits

- Locust's [official documentation](https://docs.locust.io/en/stable/index.html)
- 
