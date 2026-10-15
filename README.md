# Linux privilege escalation lab: a setuid binary, a sudo rule and a writable cron job

[![check](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml/badge.svg)](https://github.com/mitsurutech/lab-linux-privesc/actions/workflows/check.yml)

A ready-made **Linux privilege escalation lab** for classrooms, from the [MitsuruTech Learning Platform](https://mitsurutech.co.uk/learning), where schools and colleges give every student their own machines and terminal in the browser. Each student gets their own Ubuntu VM with a second, low-privilege account already on it, `intern`, and three ordinary administrator mistakes seeded for them to find: a helper program carrying the setuid bit it was never meant to have, a sudo rule that lets `intern` run one editor as root with no password, and a maintenance script root's own cron runs every minute that anybody can write to. The task is to use one of the three to reach root and read `/root/flag.txt`.

This is a security exercise, so the one rule comes first: attack only your own VM. It has no route to any other student's machine and no route to the internet beyond Ubuntu's own package archive, PyPI, npm and GitHub. Never point anything learned here at a machine that is not your own: under the Computer Misuse Act 1990, doing so without permission is a criminal offence.

## The lab

| File | What the student does | Help page |
| --- | --- | --- |
| [`linux-privesc.lab.yaml`](linux-privesc.lab.yaml) | Switch to the low-privilege account `intern`, find one of three seeded weaknesses, use it to reach root, and prove it | [Linux privilege escalation lab](https://mitsurutech.co.uk/help/lab-linux-privesc) |

The setup leaves a README in the student's home folder that says the rule, names the second account and its password, and says that root has left a way in. It gives no exploitation steps. The check marks a handed-in VM without the teacher reading it: it runs as root on a copy of the saved disk after a fresh boot, and it passes only when `/home/intern/proof.txt` matches the real flag, `/home/intern/report.txt` names a weakness, both files are owned by `intern` rather than by the VM's own built-in administrator account, and the three weaknesses are still in place exactly as seeded, because removing one afterwards would be erasing the evidence of how root was reached rather than fixing anything.

## What is in the file

`linux-privesc.lab.yaml` is a whole **VM lab definition**, exactly what the platform runs:

| Key | What it is |
| --- | --- |
| `cpus`, `memory`, `disk` | the size of the student's VM: 1 CPU, 1024 MB and 6 GB |
| `open` | the ports that open in the browser: none, the lab is the terminal |
| `setup` | a script run once, as root, the first time the VM starts: it creates the `intern` account, writes the flag, seeds the three weaknesses, and writes the README |
| `check` | the marking script, run as root on a copy of the saved disk after a fresh boot; its exit status is the result and `0` passes |

## Using it on the platform

**To set it as work for a class:**

1. In your module, add a task and choose the **Ubuntu VM lab** card.
2. Press the sample to start from, or paste the contents of `linux-privesc.lab.yaml`. Using a sample replaces the text, and Undo brings yours back.
3. Publish and try your own copy first. Every student gets a VM of their own, kept between sessions.
4. Assign it like any task. When it is handed in, run the check on a student's row in the class list: it starts a copy of their saved machine, not saved back, and shows whether it passed and what it printed.

The help manual's [guide to Ubuntu VM labs](https://mitsurutech.co.uk/help/labs) and its [sample labs page](https://mitsurutech.co.uk/help/sample-labs) cover the rest. No college account yet? [Your first year is free](https://mitsurutech.co.uk/pricing) for up to 50 students, and [signing up](https://mitsurutech.co.uk/signup) takes an email address.

## Checking the file

On every push, `.github/workflows/check.yml` checks that the file is valid YAML with the five keys above and that `setup` and `check` are each inside the platform's 16 KB limit. It does not boot a VM: that is done on the platform's own machines.

## What does not work in a lab

The VM reaches Ubuntu's package archive, PyPI, npm and GitHub, and nothing else, so there is nothing on the far side of this lab to attack and no second machine to attack from. It is one VM with one extra account on it.

## About

Made by [Mitsuru Technologies](https://mitsurutech.co.uk) for the [MitsuruTech Learning Platform](https://mitsurutech.co.uk/learning): Python, Java, Go, Node, SQL and Linux in the browser for schools and colleges, with container labs, VM labs, web apps deployed from GitHub, and quizzes that mark themselves. Try code with no account in the [playground](https://mitsurutech.co.uk/playground).

More labs from the same place: [lab-netlab](https://github.com/mitsurutech/lab-netlab), [lab-juice-shop](https://github.com/mitsurutech/lab-juice-shop), [lab-dvwa](https://github.com/mitsurutech/lab-dvwa), [lab-webgoat](https://github.com/mitsurutech/lab-webgoat), [lab-mysql-phpmyadmin](https://github.com/mitsurutech/lab-mysql-phpmyadmin) and [lab-sample](https://github.com/mitsurutech/lab-sample).

## Licence

The lab definition and this documentation are MIT licensed, see [LICENSE](LICENSE): copy them, change them, teach with them. Everything the setup installs, `cron` and `vim`, comes from Ubuntu's own archive when the VM starts; nothing of either is distributed here.
