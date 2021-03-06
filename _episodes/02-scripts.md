---
title: Writing MATLAB Scripts
teaching: 30
exercises: 0
questions:
- "How can I save and re-use my programs?"
objectives:
- "Write and save MATLAB scripts."
- "Save MATLAB plots to disk."
keypoints:
- "Save MATLAB code in files with a `.m` suffix."
---

So far, we've typed in commands one-by-one on the command line
to get MATLAB to do things for us. But what if we want to repeat
our analysis? Sure, it's only a handful of commands,
and typing them in shouldn't take
us more than a few minutes. But if we forget a step or make a mistake,
we'll waste time rewriting commands. Also, we'll quickly find ourselves
doing more complex analyses, and we'll need our results to
be more easily reproducible.

In addition to running MATLAB commands one-by-one on the
command line, we can
also write several commands in a _script_. A MATLAB script
is just a text file with a `.m` extension. We've written
commands to load data from a `.csv` file and
displays some statistics about that data. Let's
put those commands in a script called `analyze.m`,
which we'll save in our current directory,`matlab-novice-inflammation`:

~~~
% script analyze.m

patient_data = csvread('data/inflammation-01.csv');

disp(['Analyzing "inflammation-01.csv": '])
disp(['Maximum inflammation: ', num2str(max(patient_data(:)))]);
disp(['Minimum inflammation: ', num2str(min(patient_data(:)))]);
disp(['Standard deviation: ', num2str(std(patient_data(:)))]);
~~~
{: .matlab}

You can get MATLAB to run those commands by typing in the name
of the script (without the `.m`) in the MATLAB command line:

~~~
analyze
~~~
{: .matlab}

~~~
Maximum inflammation: 20
Minimum inflammation: 0
Standard deviation: 4.6148
~~~
{: .matlab}

> ## The MATLAB path
> MATLAB knows about files in the current directory, but if we want to
> run a script saved in a different location, we need to make sure that
> this file is visible to MATLAB.
> We do this by adding directories to the MATLAB **path**.
> The *path* is a list of directories MATLAB will search through to locate
> files.
> 
> To add a directory to the MATLAB path,
> we go to the `Home` tab,
> click on `Set Path`,
> and then on `Add with Subfolders...`.
> We navigate to the directory and
> add it to the path to tell MATLAB where to look for our files. When you refer
> to a file (either code or data), MATLAB will search all the directories in the path
> to find it. Alternatively, for data files, we can provide the relative or
> absolute file path.
{: .callout}

> ## GNU Octave
>
> Octave has only recently gained a MATLAB-like user interface. To change the
> path in any version of Octave, including command-line-only installations, use
> `addpath('path/to/directory')`
{: .callout}

We've also written commands to create plots, so let's include those in our script too,
but this time we'll save the figures to disk as image files using the `print` command.
In order to maintain an organised project we'll save the images
in the `results` directory:

~~~
% Plot average inflammation per day
plot(ave_inflammation);
title('Daily average inflammation')
xlabel('Day of trial')
ylabel('Inflammation')

% Save plot in 'results' folder as png image:
print('results/average','-dpng')
~~~
{: .matlab}

You might have noticed that we described what we want
our code to do using the percent sign: `%`.
This is another plus of writing scripts: you can comment
your code to make it easier to understand when you come
back to it after a while.

We can combine multiple plots into one figure using the `subplot` command
which plots our graphs in a grid pattern.
The first two parameters describe the grid we want to use, while the third
parameter indicates the placement on the grid.

~~~
% script analyze.m

patient_data = csvread('data/inflammation-01.csv');

disp(['Maximum inflammation: ', num2str(max(patient_data(:)))]);
disp(['Minimum inflammation: ', num2str(min(patient_data(:)))]);
disp(['Standard deviation: ', num2str(std(patient_data(:)))]);

ave_inflammation = mean(patient_data, 1);

subplot(1, 3, 1)
plot(ave_inflammation)
title('Average')
ylabel('Inflammation')
xlabel('Day')

subplot(1, 3, 2)
plot(max(patient_data, [], 1))
title('Max')
ylabel('Inflammation')
xlabel('Day')

subplot(1, 3, 3)
plot(min(patient_data, [], 1))
title('Min')
ylabel('Inflammation')
xlabel('Day')

% Save plot in 'results' directory as png image.
print('results/patient_data-01','-dpng')
~~~
{: .matlab}

When saving plots to disk,
it's sometimes useful to turn off their visibility as MATLAB plots them.
For example, we might not want to view (or spend time closing) the figures in MATLAB, and
not displaying the figures could make the script run faster.

Let's add a couple of lines of code to do this:

~~~
% script analyze.m

patient_data = csvread('data/inflammation-01.csv');

disp(['Maximum inflammation: ', num2str(max(patient_data(:)))]);
disp(['Minimum inflammation: ', num2str(min(patient_data(:)))]);
disp(['Standard deviation: ', num2str(std(patient_data(:)))]);

ave_inflammation = mean(patient_data, 1);

figure('visible', 'off')

subplot(1, 3, 1)
plot(ave_inflammation)
title('Average')
ylabel('inflammation')
xlabel('Day')

subplot(1, 3, 2)
plot(max(patient_data, [], 1))
title('Max')
ylabel('Inflammation')
xlabel('Day')

subplot(1, 3, 3)
plot(min(patient_data, [], 1))
title('Min')
ylabel('Inflammation')
xlabel('Day')
subplot(1, 3, 1)

% Save plot in 'results' directory as png image.
print('results/patient_data-01','-dpng')

close()
~~~
{: .matlab}

If we call the `figure` function without any options,
MATLAB will open up an empty figure window.
Try this on the command line:

~~~
figure()
~~~
{: .matlab}

We can ask MATLAB to create an empty figure window without
displaying it by setting its `'visible'` property to `'off'`, like so:

~~~
figure('visible', 'off')
~~~
{: .matlab}

When we do this, we have to be careful to manually "close" the figure
after we are doing plotting on it - the same as we would "close"
an actual figure window if it were open:

~~~
close()
~~~
{: .matlab}
