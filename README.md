Building a Registration Window in Kotlin with Jetpack Compose Creating a modern, clean registration window is one of the most common tasks in Android development. By leveraging Kotlin and Jetpack Compose, you can build a responsive, secure, and beautiful UI with minimal boilerplate code.

Here is a step-by-step breakdown of how a standard registration screen is structured.

Managing the State Before building the UI, we need to track what the user types. In Compose, we use mutableStateOf to handle the inputs for the username, email, password, and confirm password fields.

Crafting the UI Components A standard registration form consists of:

OutlinedTextFields: For user input, equipped with clear labels and icons.

VisualTransformation: To hide the password characters for security.

Button: To trigger the registration logic.


EXAMPLE 1 (For 2th)

```
DIALOG 2

package com.example.dialogv2

import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.background
import androidx.compose.foundation.border
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.size
import androidx.compose.foundation.shape.CircleShape
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Edit
import androidx.compose.material3.AlertDialog
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Checkbox
import androidx.compose.material3.CheckboxDefaults
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateListOf
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        setContent {
            MaterialTheme {
                DialogScreen()
            }
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun DialogScreen() {
        // Добавил состояние открытия диалогового окна
        var showDialog by remember { mutableStateOf(false) }

        // Добавил список сохраненных выбранных значений
        val selectedOptions = remember { mutableStateListOf<String>() }

        // Добавил временный список выбора внутри диалога
        val tempOptions = remember { mutableStateListOf<String>() }

        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.White)
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(top = 120.dp),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Box(
                    contentAlignment = Alignment.BottomEnd
                ) {
                    // Аватар пользователя
                    Box(
                        modifier = Modifier
                            .size(92.dp)
                            .clip(CircleShape)
                            .background(Color(0xFFDDEBFF)),
                        contentAlignment = Alignment.Center
                    ) {
                        Box(
                            modifier = Modifier
                                .size(44.dp)
                                .clip(CircleShape)
                                .background(Color(0xFFA8D4FF))
                        )

                        Box(
                            modifier = Modifier
                                .padding(top = 48.dp)
                                .size(width = 70.dp, height = 35.dp)
                                .clip(RoundedCornerShape(topStart = 35.dp, topEnd = 35.dp))
                                .background(Color(0xFFA8D4FF))
                        )
                    }

                    IconButton(
                        onClick = {
                            // Добавил открытие диалога с прошлым выбором
                            tempOptions.clear()
                            tempOptions.addAll(selectedOptions)
                            showDialog = true
                        },
                        modifier = Modifier
                            .size(30.dp)
                            .clip(CircleShape)
                            .background(Color(0xFF0D73FF))
                    ) {
                        Icon(
                            imageVector = Icons.Default.Edit,
                            contentDescription = "Редактировать",
                            tint = Color.White,
                            modifier = Modifier.size(16.dp)
                        )
                    }
                }

                Spacer(modifier = Modifier.height(18.dp))

                Text(
                    text = "Lucas Scott",
                    fontSize = 16.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color.Black
                )

                Spacer(modifier = Modifier.height(6.dp))

                Text(
                    text = "Выбрал виды диалоговых окон:",
                    fontSize = 13.sp,
                    color = Color(0xFF8A8A8A)
                )

                Spacer(modifier = Modifier.height(120.dp))

                // Добавил отображение всех сохраненных вариантов
                selectedOptions.forEach { option ->
                    Text(
                        text = option,
                        fontSize = 18.sp,
                        fontWeight = FontWeight.Bold,
                        color = Color.Black,
                        modifier = Modifier.padding(bottom = 14.dp)
                    )
                }
            }

            if (showDialog) {
                AlertDialog(
                    onDismissRequest = {
                        // Добавил закрытие диалога при нажатии вне окна
                        showDialog = false
                    },
                    confirmButton = {},
                    dismissButton = {},
                    title = null,
                    text = {
                        Column(
                            modifier = Modifier.fillMaxWidth(),
                            horizontalAlignment = Alignment.CenterHorizontally
                        ) {
                            Text(
                                text = "Выберите из списка",
                                fontSize = 18.sp,
                                fontWeight = FontWeight.Bold,
                                color = Color(0xFF222222),
                                textAlign = TextAlign.Center
                            )

                            Spacer(modifier = Modifier.height(28.dp))

                            Text(
                                text = "Выберите виды диалоговых окон:",
                                fontSize = 14.sp,
                                color = Color(0xFF8A8A8A),
                                textAlign = TextAlign.Center
                            )

                            Spacer(modifier = Modifier.height(20.dp))

                            CheckOptionRow(
                                text = "TimePickerDialog",
                                checked = tempOptions.contains("TimePickerDialog"),
                                onClick = {
                                    // Добавил выбор TimePickerDialog
                                    if (tempOptions.contains("TimePickerDialog")) {
                                        tempOptions.remove("TimePickerDialog")
                                    } else {
                                        tempOptions.add("TimePickerDialog")
                                    }
                                }
                            )

                            Spacer(modifier = Modifier.height(12.dp))

                            CheckOptionRow(
                                text = "DatePickerDialog",
                                checked = tempOptions.contains("DatePickerDialog"),
                                onClick = {
                                    // Добавил выбор DatePickerDialog
                                    if (tempOptions.contains("DatePickerDialog")) {
                                        tempOptions.remove("DatePickerDialog")
                                    } else {
                                        tempOptions.add("DatePickerDialog")
                                    }
                                }
                            )

                            Spacer(modifier = Modifier.height(12.dp))

                            CheckOptionRow(
                                text = "AlertDialog",
                                checked = tempOptions.contains("AlertDialog"),
                                onClick = {
                                    // Добавил выбор AlertDialog
                                    if (tempOptions.contains("AlertDialog")) {
                                        tempOptions.remove("AlertDialog")
                                    } else {
                                        tempOptions.add("AlertDialog")
                                    }
                                }
                            )

                            Spacer(modifier = Modifier.height(28.dp))

                            Row(
                                modifier = Modifier.fillMaxWidth(),
                                horizontalArrangement = Arrangement.SpaceEvenly
                            ) {
                                Button(
                                    onClick = {
                                        // Добавил кнопку отмены
                                        showDialog = false
                                        Toast.makeText(
                                            this@MainActivity,
                                            "Ничего не выбрано",
                                            Toast.LENGTH_SHORT
                                        ).show()
                                    },
                                    colors = ButtonDefaults.buttonColors(
                                        containerColor = Color.White
                                    ),
                                    shape = RoundedCornerShape(12.dp),
                                    modifier = Modifier
                                        .size(width = 110.dp, height = 44.dp)
                                        .border(
                                            width = 2.dp,
                                            color = Color(0xFF0D73FF),
                                            shape = RoundedCornerShape(12.dp)
                                        )
                                ) {
                                    Text(
                                        text = "Отмена",
                                        color = Color(0xFF0D73FF),
                                        fontWeight = FontWeight.Bold
                                    )
                                }

                                Button(
                                    onClick = {
                                        // Добавил сохранение выбранных значений
                                        if (tempOptions.isEmpty()) {
                                            Toast.makeText(
                                                this@MainActivity,
                                                "Ничего не выбрано",
                                                Toast.LENGTH_SHORT
                                            ).show()
                                        } else {
                                            selectedOptions.clear()
                                            selectedOptions.addAll(tempOptions)
                                            showDialog = false
                                        }
                                    },
                                    colors = ButtonDefaults.buttonColors(
                                        containerColor = Color.White
                                    ),
                                    shape = RoundedCornerShape(12.dp),
                                    modifier = Modifier
                                        .size(width = 110.dp, height = 44.dp)
                                        .border(
                                            width = 2.dp,
                                            color = Color(0xFF0D73FF),
                                            shape = RoundedCornerShape(12.dp)
                                        )
                                ) {
                                    Text(
                                        text = "Сохранить",
                                        color = Color(0xFF0D73FF),
                                        fontWeight = FontWeight.Bold
                                    )
                                }
                            }
                        }
                    },
                    containerColor = Color.White,
                    shape = RoundedCornerShape(18.dp),
                    modifier = Modifier.padding(horizontal = 16.dp)
                )
            }
        }
    }

    @Composable
    fun CheckOptionRow(
        text: String,
        checked: Boolean,
        onClick: () -> Unit
    ) {
        Row(
            modifier = Modifier
                .fillMaxWidth()
                .height(46.dp)
                .clip(RoundedCornerShape(16.dp))
                .background(
                    if (checked) Color(0xFFE7F0FF)
                    else Color.White
                )
                .border(
                    width = 1.dp,
                    color =
                        if (checked) Color(0xFFE7F0FF)
                        else Color(0xFFE0E0E0),
                    shape = RoundedCornerShape(16.dp)
                )
                .clickable {
                    onClick()
                }
                .padding(horizontal = 12.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            Checkbox(
                checked = checked,
                onCheckedChange = {
                    onClick()
                },
                colors = CheckboxDefaults.colors(
                    checkedColor = Color(0xFF6D5A9A),
                    uncheckedColor = Color(0xFF6A6A6A),
                    checkmarkColor = Color.White
                )
            )

            Text(
                text = text,
                fontSize = 15.sp,
                fontWeight = FontWeight.Bold,
                color = Color(0xFF222222),
                modifier = Modifier.padding(start = 12.dp)
            )
        }
    }
}
```
Creating a Registration Screen in Kotlin using XML and ViewModel
While Jetpack Compose is the future of Android, the traditional XML View system combined with View Binding and a ViewModel remains a robust, industry-standard way to build a registration window. This approach ensures a clean separation of concerns between your UI and your business logic.

1. The Architecture (MVVM)
To make the registration window scalable and testable, we divide the responsibility:

XML Layout: Defines the visual structure (EditTexts, Buttons).

Activity/Fragment: Handles UI interactions and observes data.

ViewModel: Validates inputs (e.g., checking if the email is valid or if passwords match) and communicates with the backend.

Step-by-Step Implementation
The XML Layout (activity_register.xml)
We use TextInputLayout from the Material Components library to get those smooth floating labels and built-in error placeholders.

EXAMPLE 2 (FOR 1st)
```
DIALOG 1

package com.example.dialogv1

import android.os.Bundle
import android.widget.Toast
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.Image
import androidx.compose.foundation.background
import androidx.compose.foundation.border
import androidx.compose.foundation.clickable
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Box
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.Spacer
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.size
import androidx.compose.foundation.shape.CircleShape
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.material3.AlertDialog
import androidx.compose.material3.Button
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Icon
import androidx.compose.material3.IconButton
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.RadioButton
import androidx.compose.material3.RadioButtonDefaults
import androidx.compose.material3.Surface
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.draw.clip
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.graphics.ColorFilter
import androidx.compose.ui.res.painterResource
import androidx.compose.material.icons.Icons
import androidx.compose.material.icons.filled.Edit
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()

        setContent {
            MaterialTheme {
                DialogScreen()
            }
        }
    }

    @Preview(showSystemUi = true)
    @Composable
    fun DialogScreen() {
        // Добавил состояние для открытия и закрытия диалога
        var showDialog by remember { mutableStateOf(false) }

        // Добавил состояние для сохраненного выбранного значения
        var selectedOption by remember { mutableStateOf("") }

        // Добавил временное состояние для выбора внутри диалога
        var tempOption by remember { mutableStateOf("") }

        Box(
            modifier = Modifier
                .fillMaxSize()
                .background(Color.White)
        ) {
            Column(
                modifier = Modifier
                    .fillMaxSize()
                    .padding(top = 90.dp),
                horizontalAlignment = Alignment.CenterHorizontally
            ) {
                Box(
                    contentAlignment = Alignment.BottomEnd
                ) {
                    // Аватар пользователя
                    Box(
                        modifier = Modifier
                            .size(92.dp)
                            .clip(CircleShape)
                            .background(Color(0xFFDDEBFF)),
                        contentAlignment = Alignment.Center
                    ) {
                        Box(
                            modifier = Modifier
                                .size(44.dp)
                                .clip(CircleShape)
                                .background(Color(0xFFA8D4FF))
                        )

                        Box(
                            modifier = Modifier
                                .padding(top = 48.dp)
                                .size(width = 70.dp, height = 35.dp)
                                .clip(RoundedCornerShape(topStart = 35.dp, topEnd = 35.dp))
                                .background(Color(0xFFA8D4FF))
                        )
                    }

                    IconButton(
                        onClick = {
                            // Добавил открытие диалогового окна
                            tempOption = selectedOption
                            showDialog = true
                        },
                        modifier = Modifier
                            .size(30.dp)
                            .clip(CircleShape)
                            .background(Color(0xFF0D73FF))
                    ) {
                        Icon(
                            imageVector = Icons.Default.Edit,
                            contentDescription = "Редактировать",
                            tint = Color.White,
                            modifier = Modifier.size(16.dp)
                        )
                    }
                }

                Spacer(modifier = Modifier.height(18.dp))

                Text(
                    text = "Lucas Scott",
                    fontSize = 16.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color.Black
                )

                Spacer(modifier = Modifier.height(6.dp))

                Text(
                    text = "Выбран отображение даты:",
                    fontSize = 13.sp,
                    color = Color(0xFF8A8A8A)
                )

                Spacer(modifier = Modifier.height(120.dp))

                // Добавил отображение сохраненного значения на главном экране
                Text(
                    text = selectedOption,
                    fontSize = 20.sp,
                    fontWeight = FontWeight.Bold,
                    color = Color.Black
                )
            }

            if (showDialog) {
                AlertDialog(
                    onDismissRequest = {
                        // Добавил закрытие диалога при нажатии вне окна
                        showDialog = false
                    },
                    confirmButton = {},
                    dismissButton = {},
                    title = null,
                    text = {
                        Column(
                            modifier = Modifier
                                .fillMaxWidth()
                                .background(Color.White),
                            horizontalAlignment = Alignment.CenterHorizontally
                        ) {
                            Text(
                                text = "Выберите из списка",
                                fontSize = 18.sp,
                                fontWeight = FontWeight.Bold,
                                color = Color(0xFF222222),
                                textAlign = TextAlign.Center
                            )

                            Spacer(modifier = Modifier.height(28.dp))

                            Text(
                                text = "Выберите формат отображения даты:",
                                fontSize = 14.sp,
                                color = Color(0xFF8A8A8A),
                                textAlign = TextAlign.Center
                            )

                            Spacer(modifier = Modifier.height(20.dp))

                            OptionRow(
                                text = "Yearly",
                                selected = tempOption == "Yearly",
                                onClick = {
                                    // Добавил выбор Yearly
                                    tempOption = "Yearly"
                                }
                            )

                            Spacer(modifier = Modifier.height(12.dp))

                            OptionRow(
                                text = "Monthly",
                                selected = tempOption == "Monthly",
                                onClick = {
                                    // Добавил выбор Monthly
                                    tempOption = "Monthly"
                                }
                            )

                            Spacer(modifier = Modifier.height(12.dp))

                            OptionRow(
                                text = "Weekly",
                                selected = tempOption == "Weekly",
                                onClick = {
                                    // Добавил выбор Weekly
                                    tempOption = "Weekly"
                                }
                            )

                            Spacer(modifier = Modifier.height(28.dp))

                            Row(
                                modifier = Modifier.fillMaxWidth(),
                                horizontalArrangement = Arrangement.SpaceEvenly
                            ) {
                                Button(
                                    onClick = {
                                        // Добавил кнопку отмены
                                        showDialog = false
                                        Toast.makeText(
                                            this@MainActivity,
                                            "Ничего не выбрано",
                                            Toast.LENGTH_SHORT
                                        ).show()
                                    },
                                    colors = ButtonDefaults.buttonColors(
                                        containerColor = Color.White
                                    ),
                                    shape = RoundedCornerShape(12.dp),
                                    modifier = Modifier
                                        .size(width = 110.dp, height = 44.dp)
                                        .border(
                                            width = 2.dp,
                                            color = Color(0xFF0D73FF),
                                            shape = RoundedCornerShape(12.dp)
                                        )
                                ) {
                                    Text(
                                        text = "Отмена",
                                        color = Color(0xFF0D73FF),
                                        fontWeight = FontWeight.Bold
                                    )
                                }

                                Button(
                                    onClick = {
                                        // Добавил сохранение выбранного значения
                                        if (tempOption.isBlank()) {
                                            Toast.makeText(
                                                this@MainActivity,
                                                "Ничего не выбрано",
                                                Toast.LENGTH_SHORT
                                            ).show()
                                        } else {
                                            selectedOption = tempOption
                                            showDialog = false
                                        }
                                    },
                                    colors = ButtonDefaults.buttonColors(
                                        containerColor = Color.White
                                    ),
                                    shape = RoundedCornerShape(12.dp),
                                    modifier = Modifier
                                        .size(width = 110.dp, height = 44.dp)
                                        .border(
                                            width = 2.dp,
                                            color = Color(0xFF0D73FF),
                                            shape = RoundedCornerShape(12.dp)
                                        )
                                ) {
                                    Text(
                                        text = "Сохранить",
                                        color = Color(0xFF0D73FF),
                                        fontWeight = FontWeight.Bold
                                    )
                                }
                            }
                        }
                    },
                    containerColor = Color.White,
                    shape = RoundedCornerShape(18.dp),
                    modifier = Modifier.padding(horizontal = 16.dp)
                )
            }
        }
    }

    @Composable
    fun OptionRow(
        text: String,
        selected: Boolean,
        onClick: () -> Unit
    ) {
        Row(
            modifier = Modifier
                .fillMaxWidth()
                .height(46.dp)
                .clip(RoundedCornerShape(16.dp))
                .background(
                    if (selected) Color(0xFFE7F0FF)
                    else Color.White
                )
                .border(
                    width = 1.dp,
                    color =
                        if (selected) Color(0xFFE7F0FF)
                        else Color(0xFFE0E0E0),
                    shape = RoundedCornerShape(16.dp)
                )
                .clickable {
                    onClick()
                }
                .padding(horizontal = 8.dp),
            verticalAlignment = Alignment.CenterVertically
        ) {
            RadioButton(
                selected = selected,
                onClick = onClick,
                colors = RadioButtonDefaults.colors(
                    selectedColor = Color(0xFF0D73FF),
                    unselectedColor = Color(0xFFBDBDBD)
                )
            )

            Text(
                text = text,
                fontSize = 15.sp,
                fontWeight = FontWeight.Bold,
                color = Color(0xFF222222),
                modifier = Modifier.padding(start = 4.dp)
            )
        }
    }
}
```
