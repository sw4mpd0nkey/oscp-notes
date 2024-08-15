## Tmux Notes



Search through history
> Ctrl + r

### Session Management

Creating a new session, go into your working directory (for the box or the pentest) and run the following:

> tmux new -s *\<HOTSNAME|PENTESTNANE\>*

Start a new session can also be done with:

> tmux

> tmux new

> tmux new-session

Re-attach a (local) detached session
	
> tmux attach

> tmux attach-session

Re-attach an attached session (detaching it from elsewhere)
	
> tmux attach -d

> tmux attach-session -d
	

Re-attach an attached session (keeping it attached elsewhere)
	
> tmux attach

> tmux attach-session
	

Detach from currently attached session
	
> ^b d

> ^b :detach
	

List sessions
	
> ^b s

> tmux ls tmux list-sessions
	


