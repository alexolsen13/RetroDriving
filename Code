//SCRIPT PLAYER
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player: MonoBehaviour
{
    public float playerSpeed;//данная переменная диктует, на сколько быстро двигается наш объект-игрок
    private Rigidbody2D rb;//переменная, которая нужна для задачи нашему объекту простой физики для движения
    private Vector2 playerDirection;//эта переменная будет использоваться для обработки направления нашего объекта-игрока
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    // Update is called once per frame
    void Update()
    {
        float directionY = Input.GetAxisRaw("Vertical");//будет использоваться для обнаружения кнопки, которую жмет пользователь
        playerDirection = new Vector2(0, directionY).normalized;
    }
    void FixedUpdate()
    {
        rb.velocity = new Vector2(0, playerDirection.y * playerSpeed);
    }
}

//SCRIPT CAMERA MOVEMENT
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraMovement : MonoBehaviour
{
    public float cameraSpeed;//эта переменная отвечает за скорость, с которой движется камера

    // Update is called once per frame
    void Update()//функция скорости камеры
    {
        transform.position += new Vector3(cameraSpeed * Time.deltaTime, 0, 0);//скорость камеры * время 
    }
}

//SCRIPT GAME OVER
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;//для использования сцен в игре(главное меню, сама игра)

public class GameOver : MonoBehaviour
{
    public GameObject gameOverPanel;//это переменная для панели с окончанием игры
    
    // Update is called once per frame
    void Update()//функция вызова панели GameOver,если найден объект "Player" 
    {
        if(GameObject.FindGameObjectWithTag("Player")==null)
        {
            gameOverPanel.SetActive(true);//вызывается панель GameOver
        }
    }
    public void Restart()//эта функция будет вызываться, когда пользователь кликает на кнопку рестарта
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);//перезагрузка сцены, когда пользователь кликает на кнопку рестарта
    }
}

//SCRIPT LOOPING BACKGROUND
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class LoopingBackground : MonoBehaviour
{
    public float backgroundSpeed;//скорость, с которой будет двигаться наш задний фон
    public Renderer backgroundRenderer;
    
    // Update is called once per frame
    void Update()//функция, в которой скорость заднего фона(текстуры) умножается на время и скорость рендера
    {
        backgroundRenderer.material.mainTextureOffset += new Vector2(backgroundSpeed * Time.deltaTime, 0f);
    }
}

//SCRIPT OBSTACLE
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Obstacle : MonoBehaviour
{
    private GameObject player;

    // Start is called before the first frame update
    void Start()
    {
        player = GameObject.FindGameObjectWithTag("Player");
    }

    private void OnTriggerEnter2D(Collider2D collision)//эта функция отвечает за то, что когда препятствие касается нашего объекта с Border Tag (границей), то он уничтожается 
    {
        if(collision.tag=="Border")
        {
            Destroy(this.gameObject);
        }
        else if(collision.tag=="Player")//если препятствие касается объекта "player" , то уничтожается player.gameObject
        {
            Destroy(player.gameObject);
        }
    }
}

//SCRIPT PAUSE MENU
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;//библиотека для управления сценами в игре(главное меню, сама игра и т.п)

public class PauseMenu : MonoBehaviour
{
    [SerializeField] GameObject pauseMenu;

   public void Pause() //функция паузы
    {
        pauseMenu.SetActive(true);
        Time.timeScale = 0f; // таймскейл = 0 = игра замораживается
    }

    public void Resume() //функция возвращения в игру
    {
        pauseMenu.SetActive(false);
        Time.timeScale = 1f; //таймскейл = 1 = игра возвращается в обычную скорость
    }

    public void Home(int sceneID)//функция вызова главного меню
    {
        Time.timeScale = 1f; //размораживает от паузы, чтобы главное меню не оставалось в заморозке как и пауза
        SceneManager.LoadScene(sceneID);// загружает в выбранный id какой-либо сцены(в в данном случае главное меню)
    }
}

//SCRIPT SCORE MANAGER
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class ScoreManager : MonoBehaviour
{
    public Text scoreText;//данная переменная будет нам показывать счетчик игрока
    private float score;//данная переменная будет хранилищем для счетчика игрока

    
    // Update is called once per frame
    void Update()
    {
        if(GameObject.FindGameObjectWithTag("Player")!=null)//если игрок не умер, то к счетчику игрока будет прибавляться 1 очко каждую 1 секунду
        {
            score += 1 * Time.deltaTime;
            scoreText.text = ((int)score).ToString();
        }
    }
}

//SCRIPT BACKGROUND MUSIC
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BackGroundMusic : MonoBehaviour
{
    private static BackGroundMusic backgroundMusic;

    void Awake()
    {
        if(backgroundMusic==null)
        {
            backgroundMusic = this;
            DontDestroyOnLoad(backgroundMusic);
        }
        else
        {
            Destroy(gameObject);
        }
    }
}

//SCRIPT SPAWN OBSTACLES
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class SpawnObstacles : MonoBehaviour
{
    public GameObject obstacle;//переменная, которую нужно спавнить в игре
    //данные 4 переменные нужны, чтобы наши препятствия в максимуме и минимуме для каждой оси
    public float maxX;
    public float minX;
    public float maxY;
    public float minY;
    public float timeBetweenSpawn;//переменная, которая будет задавать время для появления следующего препятствия
    private float spawnTime;

    
    // Update is called once per frame
    void Update()
    {
        if(Time.time>spawnTime)//Эта функция проверяет время, которое прошло и если оно больше предыдущего, то спавнится новое препятствие. 
        {
            Spawn();
            spawnTime = Time.time + timeBetweenSpawn;
        }
    }
    void Spawn()//Функция отвечает за рандомный спавн в диапозоне осей X и Y
    {
        float randomX = Random.Range(minX, maxX);
        float randomY = Random.Range(minY, maxY);

        Instantiate(obstacle, transform.position + new Vector3(randomX, randomY, 0), transform.rotation);
    }
}

//SCRIPT VOLUME VALUE
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class VolumeValue : MonoBehaviour
{
    private AudioSource audioSrc; //компонет, на который будем ссылаться
    private float musicVolume = 1f;//этим блоком сможем корректировать нашу громкость, float - для использования десятичных значений

    // Start is called before the first frame update
    void Start()
    {
        audioSrc = GetComponent<AudioSource>();//получение информации с этого компонента
    }

    // Update is called once per frame
    void Update()
    {
        audioSrc.volume = musicVolume;//audioSrc ссылается на пункт Volume
    }

    public void SetVolume(float vol)
    {
        musicVolume = vol;//этот блок будет забирать информацию со слайдера и отправлять эту информацию к нашему компаненту
    }
}
