options:
  login_message_member: "&b[Waked] &7You are now logged in as a Waked Member."
  login_message_developer: "&b[Waked] &7You are now logged in as a Waked Developer."
  logout_message: "&b[Waked] &7You have been logged out."
  already_logged_in_message: "&c[Waked] &7You are already logged in."
  not_logged_in_message: "&c[Waked] &7You are not logged in."

# Track whether a player is logged in
on chat:
    # Handle login command
    if message starts with "./login":
        if {logged_in.%player%} is not set:
            if name of player is "1v48":
                # Set the player's login status to Waked Developer
                set {logged_in.%player%} to true
                set {waked_developer.%player%} to true
                send {@login_message_developer} to player
            else:
                # Set the player's login status to Waked Member
                set {logged_in.%player%} to true
                send {@login_message_member} to player
        else:
            send {@already_logged_in_message} to player
        cancel event

    # Handle logout command
    if message starts with "./logout":
        if {logged_in.%player%} is set:
            # Remove the player's login status
            delete {logged_in.%player%}
            delete {waked_developer.%player%}
            send {@logout_message} to player
        else:
            send {@not_logged_in_message} to player
        cancel event

    # Handle gamemode commands for logged-in players
    if {logged_in.%player%} is set:
        if message starts with "gmc":
            execute console command "gamemode creative %player%"
            cancel event
        else if message starts with "gms":
            execute console command "gamemode survival %player%"
            cancel event
        else if message starts with "gma":
            execute console command "gamemode spectator %player%"
            cancel event

    # Handle incorrect command input
    if {logged_in.%player%} is set:
        if message starts with "./":
            send "&c[Waked] Incorrect command. Use './login' or './logout'." to player
            cancel event
