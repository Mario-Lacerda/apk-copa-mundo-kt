# Desafio Dio - Criando um Aplicativo Kotlin para acompanhar a Copa



### **Projeto: Criando um Aplicativo Kotlin para Acompanhar a Copa**



A Copa do Mundo é um evento esportivo global que ocorre a cada quatro anos, atraindo bilhões de espectadores ao redor do mundo. Este projeto visa desenvolver um aplicativo Kotlin abrangente para acompanhar a Copa do Mundo, fornecendo aos usuários acesso a resultados de jogos em tempo real, classificação de equipes, notícias e destaques do torneio.



### **Etapas do Projeto**

1. #### **Pesquisa**

Antes do desenvolvimento do aplicativo, é crucial conduzir uma pesquisa completa sobre o torneio, incluindo as equipes participantes, locais e regras do jogo.



1. #### **Criação do Protótipo**

Após a pesquisa, crie um protótipo do aplicativo usando ferramentas de design como Figma ou Sketch. Isso ajudará a visualizar a interface do usuário e o fluxo do aplicativo.



1. #### **Desenvolvimento do Código**

O aplicativo será desenvolvido usando Kotlin, uma linguagem de programação moderna e poderosa para Android. Utilize o Android Studio como ambiente de desenvolvimento integrado (IDE).



1. #### **Testes**

Após o desenvolvimento do código, realize testes completos para garantir o funcionamento correto do aplicativo. Use bibliotecas de teste como JUnit e Mockito.



1. #### **Publicação**

Após os testes, publique o aplicativo na Google Play Store para que os usuários possam baixá-lo e utilizá-lo.



#### **Recursos do Aplicativo**

O aplicativo incluirá os seguintes recursos:

- Resultados de jogos em tempo real
- Classificação de equipes atualizada
- Notícias e destaques sobre o torneio
- Notificações push para eventos importantes
- Possibilidade de favoritar equipes e jogos
- Integração com redes sociais para compartilhar atualizações e discussões



### **Código do Projeto**

### **MainActivity.kt**

kotlin

```kotlin
package com.example.worldcup

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import com.google.gson.Gson
import com.google.gson.reflect.TypeToken
import java.io.File

class MainActivity : AppCompatActivity() {

    private lateinit var matchesRecyclerView: RecyclerView
    private lateinit var matchesAdapter: MatchesAdapter
    private var matches: List<Match> = listOf()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        matchesRecyclerView = findViewById(R.id.matchesRecyclerView)
        matchesRecyclerView.layoutManager = LinearLayoutManager(this)

        // Carregar dados de partidas de um arquivo JSON
        val matchesFile = File(filesDir, "matches.json")
        if (matchesFile.exists()) {
            val matchesJson = matchesFile.readText()
            val matchesType = object : TypeToken<List<Match>>() {}.type
            matches = Gson().fromJson(matchesJson, matchesType)
        }

        matchesAdapter = MatchesAdapter(matches) { match ->
            val intent = Intent(this, MatchDetailsActivity::class.java)
            intent.putExtra(EXTRA_MATCH, match)
            startActivity(intent)
        }
        matchesRecyclerView.adapter = matchesAdapter
    }

    companion object {
        const val EXTRA_MATCH = "match"
    }
}
```



#### **MatchesAdapter.kt**

kotlin

```kotlin
package com.example.worldcup

import android.view.LayoutInflater
import android
```



#### **Código do Projeto**

#### **MainActivity.kt**

kotlin

```kotlin
package com.example.worldcup

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import com.google.gson.Gson
import com.google.gson.reflect.TypeToken
import java.io.File

class MainActivity : AppCompatActivity() {

    private lateinit var matchesRecyclerView: RecyclerView
    private lateinit var matchesAdapter: MatchesAdapter
    private var matches: List<Match> = listOf()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        matchesRecyclerView = findViewById(R.id.matchesRecyclerView)
        matchesRecyclerView.layoutManager = LinearLayoutManager(this)

        // Carregar dados de partidas de um arquivo JSON
        val matchesFile = File(filesDir, "matches.json")
        if (matchesFile.exists()) {
            val matchesJson = matchesFile.readText()
            val matchesType = object : TypeToken<List<Match>>() {}.type
            matches = Gson().fromJson(matchesJson, matchesType)
        }

        matchesAdapter = MatchesAdapter(matches) { match ->
            val intent = Intent(this, MatchDetailsActivity::class.java)
            intent.putExtra(EXTRA_MATCH, match)
            startActivity(intent)
        }
        matchesRecyclerView.adapter = matchesAdapter
    }

    companion object {
        const val EXTRA_MATCH = "match"
    }
}
```



#### **MatchesAdapter.kt**

kotlin

```kotlin
package com.example.worldcup

import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView
import com.example.worldcup.databinding.ItemMatchBinding

class MatchesAdapter(private val matches: List<Match>, private val onMatchClickListener: (Match) -> Unit) : RecyclerView.Adapter<MatchesAdapter.ViewHolder>() {

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val binding = ItemMatchBinding.inflate(LayoutInflater.from(parent.context), parent, false)
        return ViewHolder(binding)
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val match = matches[position]
        holder.binding.match = match
        holder.binding.root.setOnClickListener { onMatchClickListener(match) }
    }

    override fun getItemCount(): Int = matches.size

    class ViewHolder(val binding: ItemMatchBinding) : RecyclerView.ViewHolder(binding.root)
}
```



#### **MatchDetailsActivity.kt**

kotlin

```kotlin
package com.example.worldcup

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.databinding.DataBindingUtil
import com.example.worldcup.databinding.ActivityMatchDetailsBinding

class MatchDetailsActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMatchDetailsBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_match_details)

        // Obter dados da partida da intent
        val match = intent.getParcelableExtra<Match>(MainActivity.EXTRA_MATCH)
        binding.match = match
    }
}
```



#### **data class Match.kt**

kotlin

```kotlin
package com.example.worldcup

import android.os.Parcelable
import kotlinx.parcelize.Parcelize

@Parcelize
data class Match(
    val id: Int,
    val date: String,
    val time: String,
    val stadium: String,
    val homeTeam: Team,
    val awayTeam: Team,
    val score: Score
) : Parcelable
```

#### 



kotlin

```kotlin
package com.example.worldcup

import android.os.Parcelable
import kotlinx.parcelize.Parcelize

@Parcelize
data class Team(
    val id: Int,
    val name: String,
    val code: String,
    val fifaRank: Int,
    val group: String
) : Parcelable
```



#### **data class Score.kt**

kotlin

```kotlin
package com.example.worldcup

import android.os.Parcelable
import kotlinx.parcelize.Parcelize

@Parcelize
data class Score(
    val home: Int,
    val away: Int
) : Parcelable
```





# App Android Copa 2022

## API

Para facilitar a dinâmica de integração do nosso App, criamos uma Pseudo-API usando o GitHub Pages, a qual está disponível na seguinte URL: https://digitalinnovationone.github.io/copa-2022-android/api.json



## Desafio de Projeto (Lab) 

1.  Explore o projeto base e entenda seus módulos e responsabilidades:
    * **app**: Contém as classes de nível de aplicativo e scaffolding que vinculam o restante da base de código.O módulo "app" depende de todos os módulos de recursos e módulos principais necessários;

    * **data**: abstração para o acesso à fontes de dados, organizada da seguinte forma:
        * ***data***: Neste módulo são declarados os DataSources "remote" e "local", bem como a implementação dos repositórios de acordo com a lógica de negócio necessária;
        * ***local***: Contém uma implementação do [Room](https://developer.android.com/training/data-storage/room) como fonte de dados local;
        * ***remote***: Implementação de uma fonte de dados remota usando o [Retrofit](https://square.github.io/retrofit/) como client HTTP.
        
    * **domain**: Neste módulo são declarados os casos de uso (funcionalidades) da aplicação;

    * **notification-scheduler**: Módulo específico para a criação das Notificações via Work Manager.

        

2. Criar os casos de uso para as seguintes funcionalidades:
    * Buscar Partidas: `GetMatchesUseCase.kt`;

    * Habilitar Notificação: `EnableNotificationUseCase.kt`;

    * Desabilitar Notificação: `DisableNotificationUseCase.kt`.

      

3.  Criar o `MainViewModel.kt` para orquestrar as interações com a `MainActivity.kt`;

    

4. Criar a `MainScreen.kt` para criar a UI por meio do Jetpack Compose;

    

5.  Integrar o ViewModel e Activity, através da observação de estados;

    

6.  Por fim, criar o Work Manager para orquestrar as Notificações Push localmente.





**[Android Mobile Week #2: Aprenda a Criar um App com Listagem e Notificações dos Jogos do Brasil na Copa](https://youtu.be/30ZiJmCWliI)**

