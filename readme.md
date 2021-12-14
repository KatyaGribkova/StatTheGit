# StatTheGit - Python Code to Maintain Statistics for GitHub Repositories
![StatTheGit](images/statthegit.png)

## What is StatTheGit
StatTheGit is a python based tool to fetch, maintain and display GitHub clone and views statistics. By default GitHub only maintains GitHub repository statistics
for 14 days only. This repository can be used to maintain a local copy of the repository statistics.

***This project was forked from [StatTheGit](https://github.com/aqeelanwar/StatTheGit) repo, with a few fixes added for fetch_stats.py***

## Detailed Documentation:
Detailed documentation on this repository can be found [here](https://medium.com/@aqeel.anwar/maintaining-github-stats-for-more-than-14-days-31653bd1d7e1?sk=0d4a7e0c1b21df8a6e715719109dcecc)
## How to use StatTheGit:

### Clone the repository
```
git clone <repo_name>
```

### Install required packages
```
cd StatTheGit
pip install -r requirements.txt
```

### Fetch the stats
```
python fetch_stats.py --GitToken <GitToken> --username <GitHub Username> --RepoNames <Repository name>
```

|  Argument 	|                                                                  Explanation                                                                 	|
|:---------:	|:--------------------------------------------------------------------------------------------------------------------------------------------:	|
|  GitToken 	|                                                         GitHub personal access token                                                         	|
|  username 	|                                                              The Github username                                                             	|
| RepoNames 	| Name of the repository or repositories separated by space. If set to "all", all the repositories under the username profile will be processed; if set to "allpush", all repositories that user has push access to will be processed. |

Running fetch_stats.py will create a folder repo_stats/<-username->. The view and clone stats for the mentioned repositories will be fetched from the GitHub profile and saved as a csv file. If the csv files for the repository already exists the code appends the fetched data to existing stats taking care of issues such as duplicate stats, missing dates etc.

```
# Generic
|-- repo_stats
|    |-- <username>
|    |    |-- <repository 1>.txt
|    |    |-- <repository 2>.txt
|    |    |-- <repository 3>.txt


# Example
|-- repo_stats
|    |-- aqeelanwar
|    |    |-- PEDRA.txt
|    |    |-- SocialDistancingAI.txt
```

### Display the stats
StatTheGit can be used to create and update online graphs which can then be displayed on your personal website [like this](http://www.aqeel-anwar.com/#GitHubStat).

#### Offline graphs:

```
python display_stats.py --stat_folder repo_stats --display_type 'offline'
```

Running the above command will generate one interactive graph per repository displaying the views and clones statistics.

#### Online Graphs:
Plotly is being used to plot the Github graphs. In order to create the graphs online, and have it displayed on your website, chart studio account needs to be created [Details here](https://medium.com/@aqeel.anwar/maintaining-github-stats-for-more-than-14-days-31653bd1d7e1?sk=0d4a7e0c1b21df8a6e715719109dcecc). Once you have the API key you can use the following commands to create online graphs that can then be shared on websites.
```
python display_stats.py --stat_folder repo_stats --display_type 'online' --username <plotly-username> --api_key <plotly-api-key
```


#### A Way to Schedule a Weekly Stats Fetch (tested on Windows 10):
Create a batch file (ex: Get_GitHub_Stats.bat), preferably in the same directory as fetch_stats.py, with specified options. Template with placeholders:
```
call <anaconda_dir>\Scripts\activate.bat <EnvironmentName> 
call python fetch_stats.py --GitToken <GitToken> --username <GitHub Username> --RepoNames <Repository Names>
```
If StatTheGit module was not installed in a separate Python environment, ignore the first line of the batch code.

Windows 10 has a Task Scheduler tool. Create a new task that launches the batch file with a weekly trigger (more details here: https://www.windowscentral.com/how-create-automated-task-using-task-scheduler-windows-10). This should update files in the repo-stats directory on a weekly basis.
