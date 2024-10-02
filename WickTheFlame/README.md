# Wick The Flame

```
Duration:   2023 (1 week) — 2nd Project
Engine:     Unity
Genre:      2D Platformer
Team:       3 Programmers, 2 Artists
```

You lost your little brother in a dark cave... find him! <br> <br> Wick the Flame is a 2D Platformer that was made in a very short timeframe. We wanted to focus on the aspect of minimal light in a dark environment without making it too bothersome. <br> <br> Below, you'll find some examples of what I worked on during this project.

<table>
  <tr>
    <td width="50%"><img src="/Gifs/WickTheFlame_3.gif" /></td>
    <td width="50%"><img src="/Gifs/WickTheFlame_2.gif" /></td>
  </tr>
</table>

---

| Crystal Lights | Example |
| --- | --- |
| At first we had two main sources of light; a lighter that is held and a checkpoint system in the shape of lanterns that lights up when passed by. <br> <br> We wanted a bit more than that. So I thought crystals would be quite interesting and fitting for a cave. They always have a slight glow, but when the character moves by them, they begin shining. <br> <br> Not only did this help with locating which paths the player has already tried, but it also allowed us to make quite some cinematic moments such as the attached gif or the gif right above this text. | <img src="/Gifs/WickTheFlame_4.gif" alt="References" width="400" height="auto"> |

<details>
<summary>CrystalLights.cs</summary>

```cs
using UnityEngine;
using UnityEngine.Experimental.Rendering.Universal;

public class CrystalLights : MonoBehaviour
{
    private Light2D glow;
    private Light2D shine;

    void Start()
    {
        glow = GetComponent<Light2D>();
        glow.intensity = 0.1f;

        shine = transform.Find("Shine")?.GetComponent<Light2D>();

        if (shine != null)
            shine.intensity = 0;
    }

    private void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("Player"))
        {
            glow.intensity = 0.5f;

            if (shine != null)
                shine.intensity = 2f;
        }
    }
}

```
</details>

---

| Player Movement | Example |
| --- | --- |
| I made a script that handles the player movement including walking, jumping, and gravity control. <br> <br> It uses two ground checks, one for each feet. It adjusts the player's sprite to face the correct direction and adds dynamic features like faster falls to make the movement feel responsive. | <img src="/Gifs/WickTheFlame_5.gif" alt="References" width="400" height="auto"> |
| The character wears a red poncho. I thought it would be fun if she could use it as a way to float, which gives the player more control if they're going for a big jump. <br> <br> I made it work by simply reducing her gravity if the player presses spacebar while not grounded, and used a bool to keep track of things. | <img src="/Gifs/WickTheFlame_2.gif" alt="References" width="400" height="auto"> |

<details>
<summary>PlayerMovement.cs</summary>

```cs
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    [SerializeField] private Transform groundCheck1;
    [SerializeField] private Transform groundCheck2;
    [SerializeField] private LayerMask groundLayer;
    [SerializeField] private SpriteRenderer sprite;
    [SerializeField] private float moveSpeed = 7f;
    [SerializeField] private float jumpForce = 10f;
    [SerializeField] private float groundCheckRadius = 0.1f;

    private float dirX = 0f;
    private bool isGrounded;
    private bool isFloating;
    private bool isFacingLeft = false;
    private Rigidbody2D rb2D;
    private AudioSource audioSource;

    void Start()
    {
        rb2D = GetComponent<Rigidbody2D>();
        sprite = GetComponent<SpriteRenderer>();
        audioSource = GetComponent<AudioSource>();
    }

    void Update()
    {
        HorizontalMovement();
        HandleJump();
        AdjustGravity();
    }

    private void HandleJump()
    {
        isGrounded = Physics2D.OverlapCircle(groundCheck1.position, groundCheckRadius, groundLayer) ||
                     Physics2D.OverlapCircle(groundCheck2.position, groundCheckRadius, groundLayer);

        if (Input.GetButtonDown("Jump") && isGrounded)
        {
            rb2D.velocity = new Vector2(rb2D.velocity.x, jumpForce);
            audioSource.Play();
        }

        else if (Input.GetButtonUp("Jump") && rb2D.velocity.y > 0)
            rb2D.velocity = new Vector2(rb2D.velocity.x, rb2D.velocity.y * 0.25f);

        else if (Input.GetButtonDown("Jump") && !isGrounded)
        {
            rb2D.gravityScale = 0.2f;
            isFloating = true;
        }
    }

    private void HorizontalMovement()
    {
        dirX = Input.GetAxisRaw("Horizontal");
        rb2D.velocity = new Vector2(dirX * moveSpeed, rb2D.velocity.y);

        if (dirX > 0 && isFacingLeft)
            Flip();

        else if (dirX < 0 && !isFacingLeft)
            Flip();
    }

    private void Flip()
    {
        Vector3 playerScale = transform.localScale;
        playerScale.x *= -1;
        transform.localScale = playerScale;

        isFacingLeft = !isFacingLeft;
    }

    private void AdjustGravity()
    {
        if (rb2D.velocity.y < 0 && !isFloating)
            rb2D.gravityScale = 2.5f;

        else if (isGrounded)
        {
            rb2D.gravityScale = 1f;
            isFloating = false;
        }
    }
}

```
</details>
