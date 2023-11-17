# Duplicate Management

### Installation instructions
#### Preparation
**We assume that your odoo environment is already configured correctly**
- Make sure your DB user has privileges to create functions and extensions
- Install boolean_parser for python env, use ```pip install boolean_parser```. You may need to downgrade greenlet package to version 0.4.17 after installing boolean_parser, use ```pip install greenlet==0.4.17```
- Place the files of the following modules to the odoo modules directory
    - [queue_job](https://github.com/OCA/queue/tree/15.0/queue_job)
    - [upgrade_hook](https://gitlab.com/gbc-team/odoo/upgrade_hook)
- Place the files of the Duplication management module to the odoo modules directory
- Restart odoo application - this is necessary to start background runners

#### Installation and configuration
- In the odoo UI go to Apps and select Administration category
- Find the Duplication management module and click Install. Dependent modules will be installed automatically
- Add the following parameters to config:
    - [options]
        - server_wide_modules = web,queue_job
        - max_cron_threads = 2
        - workers = 4
    - [queue_job]
        - channels = root:2,root.system:1,root.user:1
    - [deduplication]
        - job_batch_size = 480
        - thread_count = 8
        - thread_batch_size = 60

thread_count must be less than or equal to the number of CPU cores. By default, this value is set as the number of cores.
thread_batch_size must be less than or equal job_batch_size
Excellent ratio: job_batch_size = thread_count * thread_batch_size

Average search result for 10,000 objects is 60 seconds per one CPU core
