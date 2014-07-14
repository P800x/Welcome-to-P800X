Browse to the repository in Github.com and click "Fork".

Clone your new forked repo:

	$ git clone https://github.com/<your_username>/<repo_name>.git

Configure your forked repo:

	$ git remote add upstream https://github.com/<orig_username>/<repo_name>.git

Pull in Upstream Changes

	$ git fetch upstream
	$ git merge upstream/master

To Create a Pull Request, browse to your forked repository in Github.com and click on the green "Compare, review, create a pull request" button.