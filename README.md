package com.example.connect

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.*
import androidx.compose.material3.*
import androidx.compose.runtime.*
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.unit.dp

data class User(
    val name: String,
    val lastMessage: String
)

class MainActivity : ComponentActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContent {
            MaterialTheme {
                ConnectApp()
            }
        }
    }
}

@Composable
fun ConnectApp() {

    var selectedTab by remember {
        mutableIntStateOf(0)
    }

    var selectedUser by remember {
        mutableStateOf<User?>(null)
    }

    if (selectedUser != null) {

        ChatScreen(
            user = selectedUser!!,
            onBack = {
                selectedUser = null
            }
        )

        return
    }

    Scaffold(

        topBar = {
            TopAppBar(
                title = {
                    Text(
                        when (selectedTab) {
                            0 -> "Connect"
                            1 -> "Search"
                            2 -> "Profile"
                            else -> "Settings"
                        }
                    )
                },

                actions = {

                    if (selectedTab == 0) {
                        IconButton(onClick = {}) {
                            Icon(
                                Icons.Default.Add,
                                contentDescription = "New Chat"
                            )
                        }
                    }
                }
            )
        },

        bottomBar = {

            NavigationBar {

                NavigationBarItem(
                    selected = selectedTab == 0,
                    onClick = {
                        selectedTab = 0
                    },
                    icon = {
                        Icon(
                            Icons.Default.Chat,
                            contentDescription = "Chat"
                        )
                    },
                    label = {
                        Text("Chat")
                    }
                )

                NavigationBarItem(
                    selected = selectedTab == 1,
                    onClick = {
                        selectedTab = 1
                    },
                    icon = {
                        Icon(
                            Icons.Default.Search,
                            contentDescription = "Search"
                        )
                    },
                    label = {
                        Text("Search")
                    }
                )

                NavigationBarItem(
                    selected = selectedTab == 2,
                    onClick = {
                        selectedTab = 2
                    },
                    icon = {
                        Icon(
                            Icons.Default.Person,
                            contentDescription = "Profile"
                        )
                    },
                    label = {
                        Text("Profile")
                    }
                )

                NavigationBarItem(
                    selected = selectedTab == 3,
                    onClick = {
                        selectedTab = 3
                    },
                    icon = {
                        Icon(
                            Icons.Default.Settings,
                            contentDescription = "Settings"
                        )
                    },
                    label = {
                        Text("Settings")
                    }
                )
            }
        }

    ) { padding ->

        Box(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
        ) {

            when (selectedTab) {

                0 -> ChatList(
                    onUserClick = {
                        selectedUser = it
                    }
                )

                1 -> SearchScreen()

                2 -> ProfileScreen()

                3 -> SettingsScreen()
            }
        }
    }
}


@Composable
fun ChatList(
    onUserClick: (User) -> Unit
) {

    val users = listOf(

        User(
            "Rahim",
            "Hello, how are you?"
        ),

        User(
            "Karim",
            "See you tomorrow."
        ),

        User(
            "Sakib",
            "Can you call me?"
        ),

        User(
            "Nusrat",
            "Good morning!"
        )
    )

    LazyColumn(
        modifier = Modifier.fillMaxSize()
    ) {

        items(users) { user ->

            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .clickable {
                        onUserClick(user)
                    }
                    .padding(16.dp),

                verticalAlignment = Alignment.CenterVertically
            ) {

                Icon(
                    Icons.Default.AccountCircle,
                    contentDescription = null,
                    modifier = Modifier.size(55.dp)
                )

                Spacer(
                    modifier = Modifier.width(12.dp)
                )

                Column {

                    Text(
                        text = user.name,
                        fontWeight = FontWeight.Bold
                    )

                    Text(
                        text = user.lastMessage
                    )
                }
            }
        }
    }
}


@Composable
fun ChatScreen(
    user: User,
    onBack: () -> Unit
) {

    var message by remember {
        mutableStateOf("")
    }

    Scaffold(

        topBar = {

            TopAppBar(

                title = {
                    Text(user.name)
                },

                navigationIcon = {

                    IconButton(
                        onClick = onBack
                    ) {
                        Icon(
                            Icons.Default.ArrowBack,
                            contentDescription = "Back"
                        )
                    }
                },

                actions = {

                    IconButton(onClick = {}) {
                        Icon(
                            Icons.Default.Call,
                            contentDescription = "Audio Call"
                        )
                    }

                    IconButton(onClick = {}) {
                        Icon(
                            Icons.Default.VideoCall,
                            contentDescription = "Video Call"
                        )
                    }
                }
            )
        }

    ) { padding ->

        Column(
            modifier = Modifier
                .fillMaxSize()
                .padding(padding)
        ) {

            // Messages area

            LazyColumn(
                modifier = Modifier
                    .weight(1f)
                    .fillMaxWidth()
                    .padding(12.dp)
            ) {

                item {

                    Text(
                        text = "Hello! 👋",
                        modifier = Modifier
                            .padding(8.dp)
                    )

                    Text(
                        text = "How are you?",
                        modifier = Modifier
                            .padding(8.dp)
                    )
                }
            }

            // Message input

            Row(
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(8.dp),

                verticalAlignment = Alignment.CenterVertically
            ) {

                IconButton(onClick = {}) {

                    Icon(
                        Icons.Default.Add,
                        contentDescription = "Attachment"
                    )
                }

                OutlinedTextField(
                    value = message,
                    onValueChange = {
                        message = it
                    },
                    modifier = Modifier.weight(1f),
                    placeholder = {
                        Text("Message...")
                    }
                )

                IconButton(
                    onClick = {
                        if (message.isNotBlank()) {
                            message = ""
                        }
                    }
                ) {

                    Icon(
                        Icons.Default.Send,
                        contentDescription = "Send"
                    )
                }
            }
        }
    }
}


@Composable
fun SearchScreen() {

    var search by remember {
        mutableStateOf("")
    }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {

        OutlinedTextField(
            value = search,
            onValueChange = {
                search = it
            },
            modifier = Modifier.fillMaxWidth(),
            placeholder = {
                Text("Search people...")
            },
            leadingIcon = {
                Icon(
                    Icons.Default.Search,
                    contentDescription = null
                )
            }
        )
    }
}


@Composable
fun ProfileScreen() {

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(24.dp),

        horizontalAlignment = Alignment.CenterHorizontally
    ) {

        Icon(
            Icons.Default.AccountCircle,
            contentDescription = null,
            modifier = Modifier.size(100.dp)
        )

        Spacer(
            modifier = Modifier.height(16.dp)
        )

        Text(
            text = "My Profile",
            style = MaterialTheme.typography.headlineSmall
        )

        Text(
            text = "Online"
        )
    }
}


@Composable
fun SettingsScreen() {

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp)
    ) {

        Text(
            text = "Settings",
            style = MaterialTheme.typography.headlineSmall
        )

        Spacer(
            modifier = Modifier.height(20.dp)
        )

        Text(
            text = "Account",
            modifier = Modifier.padding(12.dp)
        )

        Text(
            text = "Notifications",
            modifier = Modifier.padding(12.dp)
        )

        Text(
            text = "Privacy",
            modifier = Modifier.padding(12.dp)
        )

        Text(
            text = "Dark Mode",
            modifier = Modifier.padding(12.dp)
        )
    }
}
