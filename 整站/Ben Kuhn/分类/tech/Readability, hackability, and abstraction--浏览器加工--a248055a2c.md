Suppose you’re writing a script to spin up servers for your web application.

```python
def deploy(ip):
    copy('code/', ip + ':~/code', recursive=True)
    write_template('conf/config.py', ip + ':~/config.py')
    write_template('conf/crontab', ip + ':~/.crontab')
    write_template('conf/crontab', ip + ':/etc/apache2/httpd.conf')
    run_as_root('service cron restart')
    run_as_root('service apache restart')
    post('https://pingdom.com/api/2.0/checks',
            { 'name':ip, 'host':ip, 'type':'ping' })
```

Everything is going well until you decide to split up your machines into ones that run tasks and ones that answer requests. Since you’re not sure which deploy logic you want to be shared and which you want to keep separate, you decide to start out by copy-pasting the logic:

```python
def deploy_taskrunner(ip):
    # Warning: don't forget to edit deploy_webserver as well
    copy('code/', ip + ':~/code', recursive=True)
    write_template('conf/task_config.py', ip)
    write_template('conf/crontab', ip + ':~/.crontab')
    run_as_root('service cron restart')
    post('https://pingdom.com/api/2.0/checks',
            { 'name':ip, 'host':ip, 'type':'ping' })

def deploy_webserver(ip):
    # Warning: don't forget to edit deploy_taskrunner as well
    copy('code/', ip + ':~/code', recursive=True)
    write_template('conf/web_config.py', ip)
    write_template('conf/httpd.conf',
                    ip + ':/etc/apache2/httpd.conf')
    run_as_root('service apache restart')
    post('https://pingdom.com/api/2.0/checks',
            { 'name':ip, 'host':ip, 'type':'ping' })
```

Your application hums along for a while, and everything is fine. When you need to manually deploy something, you can read the list of steps from the relevant function. When you need to tweak some config files to improve performance, you just add the code to both locations (and put up with some nagging from your coworkers).

* * *

Suddenly, disaster strikes! You release an API! This means you need to have a third type of machine in your cluster.

Since your coworkers have been nagging you about the copy-pasted code in the deploy scripts, you decide to factor out the common logic into functions.

```python
def predeploy_common(ip):
    copy_code_to(ip)
    tweak_config_files(ip)

def postdeploy_common(ip):
    run_tests_on(ip)
    setup_pingdom(ip)

def deploy_taskrunner(ip):
    predeploy_common(ip)
    write_template('conf/task_config.py', ip)
    write_template('conf/crontab',
                    ip + ':~/.crontab')
    run_as_root('service cron restart')
    postdeploy_common(ip)

def deploy_webserver(ip):
    predeploy_common(ip)
    write_template('conf/web_config.py', ip)
    write_template('conf/httpd_web.conf',
                    ip + ':/etc/apache2/httpd.conf')
    run_as_root('service apache restart')
    postdeploy_common(ip)

def deploy_apiserver(ip):
    predeploy_common(ip)
    write_template('conf/api_config.py', ip)
    write_template('conf/httpd_api.conf',
                    ip + ':/etc/apache2/httpd.conf')
    run_as_root('service apache restart')
    postdeploy_common(ip)
```

Everything is glorious again. Your machines are fruitful and multiply. Adding more new machine types is a cinch. Your application hums along for a while.

* * *

Suddenly, disaster strikes! While adding support for BSD machines in addition to Linux, you realize that you have to change a function three levels deep inside `tweak_config_files`.

You snottily point out to your coworkers that if they had let you keep the copy-paste code, you could have just added an `if...then` block and been done with it. Your coworkers tell you to stop being ridiculous.

You briefly consider threading a `bsd=True` parameter through three levels of function calls before deciding that you’ve taken enough beatings in code review. Instead, you realize that you just need to think of your machines as objects with behaviors instead of passive IP address strings that just have stuff done to them. So you refactor your code to be object-oriented:

```python
class Machine(object):
    # abstract class
    __metaclass__ = abc.ABCMeta
    def __init__(self, ip):
        self.ip = ip
    def copy_files(self, files):
        ...
    @abc.abstractmethod
    def tweak_ram_config(self):
        pass
    @abc.abstractmethod
    def tweak_fs_config(self):
        pass
    @abc.abstractmethod
    def tweak_network_config(self):
        pass
    def tweak_config_files(self):
        self.tweak_ram_config()
        self.tweak_fs_config()
        self.tweak_network_config()
    ...

class LinuxMachine(Machine):
    ...

class BSDMachine(Machine):
    ...

class DeploymentPlan(object):
    __metaclass__ = abc.ABCMeta
    @abc.abstractmethod
    def deploy(machine):
        pass
    ...

class APIDeploymentPlan(DeploymentPlan):
    ...

class WebDeploymentPlan(DeploymentPlan):
    ...

class TaskDeploymentPlan(DeploymentPlan):
    ...

...
```

Everything is glorious again. Your machines are fruitful and multiply. When you add more different hardware types, you can just add more subclasses. Your application hums along for a while.

* * *

Suddenly, disaster strikes! [A botched deploy causes your machines to run amok, losing your app hundreds of millions of dollars before anyone figures out how to turn it off off.](http://en.wikipedia.org/wiki/Knight_Capital_Group#2012_stock_trading_disruption) Just kidding—it’s not that hard to turn off a computer. But it was a near thing.

Shaken, you drag yourself to the incident post-mortem. “What happened here?” your boss asks.

Well, you say, the deploy script started by constructing a `Machine` subclass for each machine in the cluster, and a `DeploymentPlan` instance to manage deploying to it… After a couple hours of jumping between files and trying to do [vtable lookups](http://en.wikipedia.org/wiki/Virtual_function) in your head, you eventually trace the bug to a bad assumption in a `DeploymentPlan` subclass method—the author was confused about which subtype of `Machine` they were operating on.

Your boss groans. “Didn’t this stuff used to be, like, a function in a one-file script?”

* * *

This is a circle I often find myself going around. I start out with the equivalent of copy-paste code (or a single script with if-then statements). It’s not abstract or [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself), but you can read it from beginning to end and you can change whatever part of it you want.

As the code gets more complex, I’ll decide that to keep it manageable I need to split it up into a hierarchy of functions. It always feels refreshing to abstract away a hairball of code—until I try to modify part of the internals, and realize I have to thread an argument through too many layers of functions.

At that point, I might decide to take the state that gets threaded through functions, and encapsulate it in an object. But now each method has access to a huge amount of hidden state (in the form of instance variables), and each method call could mean one of many different things depending on the runtime class of the object it’s being called on. That makes it much harder to look at code and know what path will be executed, so the code becomes much harder to keep in my head.

Copy-paste code is readably and hackable but poorly-abstracted. Large trees of functions are readable and abstract, but hard to hack. Stateful objects and virtual dispatch are both hackable and abstract, but difficult to understand. I haven’t found any pattern that accomplishes all three at once.

* * *

In fact, I wonder if it’s even possible to get all three of readability, hackability and abstraction. When I cycle through these three patterns, it feels like I’m playing code complexity whack-a-mole. As soon as I simplify one part of the program, another one gets hairier.

This might just mean the problem I’m trying to solve is inherently complex. If I’m trying to run five different deploy recipes across four different hardware/OS configurations, that’s 20 different potential interactions to take care of. At that point, unless I explicitly take stock and notice that my problem has a certain amount of inherent complexity, I’m likely to oscillate several times between different ways of expressing the same thing.

At the same time, I still hold out hope. Someday, I tell myself, I’ll write a nontrivial program that’s readable, hackable _and_ abstract. Hopefully before I blow anyone up with a botched deploy.