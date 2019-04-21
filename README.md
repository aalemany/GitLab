# GitLab
Tricks

# How to relocate Omnibus-GitLab based repositories instance, i.e. if space on the original location /var/opt/gitlab/git-data is full.
Steps to follow.
1-Decide new location for folder repositories originaly under /var/opt/gitlab/git-data/repositories
i.e. /local/git-data
2-Stopping service
sudo gitlab-ctl stop
3-Rsync to copy current repositories in production to new location
sudo rsync -avh --progress /var/opt/gitlab/git-data/repositories /local/git-data/ #there is _no_ slash behind 'repositories', but there _is_ a slash'/' behind 'git-data'.
4-Fix any rights/ownership on the new location
i.e. chown -R git:root ./local/git-data
5-Modify NEW repositories location on the gitlab conf file  /etc/gitlab/gitlab.rb
git_data_dirs({
    "default" => {
        "path" => "/local/git-data"
    }
})
6-Reconfigure GitLab 
sudo gitlab-ctl reconfigure
Start GitLab service 
sudo gitlab-ctl start

# source:  https://stackoverflow.com/questions/19902417/change-the-data-directory-gitlab-to-store-repos-elsewhere

