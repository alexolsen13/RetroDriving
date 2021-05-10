# RetroDriving
//Camera Movement
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraMovement : MonoBehaviour
{
    public float cameraSpeed;

    // Update is called once per frame
    void Update()
    {
        transform.position += new Vector3(cameraSpeed * Time.deltaTime, 0, 0);
    }
}

//Obstacle
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

    private void OnTriggerEnter2D(Collider2D collision)
    {
        if(collision.tag=="Border")
        {
            Destroy(this.gameObject);
        }
        else if(collision.tag=="Player")
        {
            Destroy(player.gameObject);
        }
    }
}

//Background music
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

//Game Over
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameOver : MonoBehaviour
{
    public GameObject gameOverPanel;
    
    // Update is called once per frame
    void Update()
    {
        if(GameObject.FindGameObjectWithTag("Player")==null)
        {
            gameOverPanel.SetActive(true);
        }
    }
    public void Restart()
    {
        SceneManager.LoadScene(SceneManager.GetActiveScene().name);
    }
}

//LoopingBackground
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class LoopingBackground : MonoBehaviour
{
    public float backgroundSpeed;
    public Renderer backgroundRenderer;
    
    // Update is called once per frame
    void Update()
    {
        backgroundRenderer.material.mainTextureOffset += new Vector2(backgroundSpeed * Time.deltaTime, 0f);
    }
}

//Pause Menu
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;

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

//Player
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Player: MonoBehaviour
{
    public float playerSpeed;
    private Rigidbody2D rb;
    private Vector2 playerDirection;
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    // Update is called once per frame
    void Update()
    {
        float directionY = Input.GetAxisRaw("Vertical");
        playerDirection = new Vector2(0, directionY).normalized;
    }
    void FixedUpdate()
    {
        rb.velocity = new Vector2(0, playerDirection.y * playerSpeed);
    }
}

//ScoreManager
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class ScoreManager : MonoBehaviour
{
    public Text scoreText;
    private float score;

    
    // Update is called once per frame
    void Update()
    {
        if(GameObject.FindGameObjectWithTag("Player")!=null)
        {
            score += 1 * Time.deltaTime;
            scoreText.text = ((int)score).ToString();
        }
    }
}

//SpawnObstacles
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class SpawnObstacles : MonoBehaviour
{
    public GameObject obstacle;
    public float maxX;
    public float minX;
    public float maxY;
    public float minY;
    public float timeBetweenSpawn;
    private float spawnTime;

    
    // Update is called once per frame
    void Update()
    {
        if(Time.time>spawnTime)
        {
            Spawn();
            spawnTime = Time.time + timeBetweenSpawn;
        }
    }
    void Spawn()
    {
        float randomX = Random.Range(minX, maxX);
        float randomY = Random.Range(minY, maxY);

        Instantiate(obstacle, transform.position + new Vector3(randomX, randomY, 0), transform.rotation);
    }
}

//VolumeValue

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
