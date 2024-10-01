# Parenthood

| Squish and Stretch | Example |
| --- | --- |
| We wanted a bit of bounce and fluidity in our characters. So I wrote this 'Squish and Stretch' script to make them more dynamic and expressive. | <img src="/Gifs/Parenthood_SaS.gif" alt="References" width="200" height="auto"> |

<details>
<summary>SquishAndStretch.cs</summary>

```cs
public class SquishAndStretch : MonoBehaviour
{
    public Transform Sprite;
    public float Stretch = 0.1f;
    [SerializeField] private Transform squashParent;

    private Rigidbody2D _rigidbody;
    private Vector3 _originalScale;

    private void Start()
    {
        _rigidbody = GetComponent<Rigidbody2D>();
        _originalScale = Sprite.transform.localScale;

        if (!squashParent)
            squashParent = new GameObject($"_squash_{name}").transform;
    }

    private void Update()
    {
        Sprite.parent = transform;
        Sprite.localPosition = Vector3.zero;
        Sprite.localScale = _originalScale;
        Sprite.localRotation = Quaternion.identity;

        squashParent.localScale = Vector3.one;
        squashParent.position = transform.position;

        Vector3 velocity = _rigidbody.velocity;

        if (velocity.sqrMagnitude > 0.01f)
            squashParent.rotation = Quaternion.FromToRotation(Vector3.right, velocity);

        float scaleX = 1.0f + (velocity.magnitude * Stretch);
        float scaleY = 1.0f / scaleX;

        Sprite.parent = squashParent;
        squashParent.localScale = new Vector3(scaleX, scaleY, 1.0f);
    }
}


```
</details>

---

| Follow | Example |
| --- | --- |
| The first thing I worked on in this project was the Follow mechanic. When the player presses E, the parent calls out for their child, who comes running to them. <br> <br> I wanted to build this in a way that feels light-hearted but also sporadic (like a kid). The method that really nailed this for me was by using random jump intervals and force, while keeping the values quite low. Once the child has catched up with their parent, they're back to normal. <br> <br> _If you time it perfectly, the child might land on top of the parents head!_ | <img src="/Gifs/Parenthood_Follow.gif" alt="References" width="400" height="auto"> |

<details>
<summary>Follow.cs</summary>
  
```cs
public class Follow : MonoBehaviour
{
  [SerializeField] Transform groundCheck;
  [SerializeField] LayerMask groundLayer;
  [SerializeField] float groundCheckRadius = 0.1f;

  public Transform target;

  private SpriteRenderer sprite;
  private Rigidbody2D rb;

  public bool isFollowing;
  public bool isGrounded;

  public float maxSpeed = 5f;
  public float acceleration = 10f;
  public float stoppingDistance = 1f;
  public float minJumpInterval = 0.5f;
  public float maxJumpInterval = 1.5f;
  public float minJumpForce = 3f;
  public float maxJumpForce = 4.5f; 

  private float timeSinceLastJump = 0f;
  private float jumpInterval = 0f;
  private float jumpForce = 0f;

  void Start()
  {
    rb = GetComponent<Rigidbody2D>();
    sprite = GetComponentInChildren<SpriteRenderer>();
  }

  void Update()
  {
      if (Input.GetKeyDown(KeyCode.E) && !isFollowing)
          isFollowing = true;
  
      if (isFollowing)
          FollowParent();
  
      isGrounded = Physics2D.OverlapCircle(groundCheck.position, groundCheckRadius, groundLayer);
  
      timeSinceLastJump += Time.deltaTime;
  
      if (isGrounded && isFollowing && timeSinceLastJump >= jumpInterval)
      {
          Jump();
          SetRandomJumpInterval();
          SetRandomJumpForce();
      }
  }
  
  public void FollowParent()
  {
      Vector2 direction = new Vector2(target.position.x - transform.position.x, 0);
      float distance = direction.magnitude;
  
      if (distance > stoppingDistance)
      {
          float speedToApply = Mathf.Min(maxSpeed, rb.velocity.magnitude + acceleration * Time.deltaTime);
          rb.velocity = new Vector2(direction.normalized.x * speedToApply, rb.velocity.y);
  
          sprite.flipX = direction.x < 0;
      }
  }

  private void Jump()
  {
      rb.AddForce(Vector2.up * jumpForce, ForceMode2D.Impulse);
  
      timeSinceLastJump = 0f;
  }
  
  private void SetRandomJumpInterval()
  {
      jumpInterval = Random.Range(minJumpInterval, maxJumpInterval);
  }
  
  private void SetRandomJumpForce()
  {
      jumpForce = Random.Range(minJumpForce, maxJumpForce);
  }

  void OnCollisionEnter2D(Collision2D collision)
  {
      if (collision.transform == target)
          isFollowing = false;
  }
}

```
</details>

---

| Expand Light | Example |
| --- | --- |
| Continuing with the Follow function and the parent _calling_ out for their child. <br> <br> We wanted to make the call visible but still minimal, to make sure that it fits the theme that we're going for. So I went with a simple but effective increase of light around the characters. <br> <br> The light gradually increases, and once it reaches maxIntensity, it stays at that value for a very short duration and then quickly decreases. | <img src="/Gifs/Parenthood_Light.gif" alt="References" width="400" height="auto"> |

<details>
<summary>ExpandLight.cs</summary>
  
```cs
public class ExpandLight : MonoBehaviour
{
  public Light2D light2D;
  public float expansionDuration = 3f;
  public float maxIntensity = 1f;
  
  private bool isExpanding = false;

  void Update()
  {
      if (Input.GetKeyDown(KeyCode.E) && !isExpanding)
      StartCoroutine(BurstLight());
  }
    
  IEnumerator BurstLight()
  {
      isExpanding = true;
  
      light2D.intensity = 0f;
  
      float elapsedTime = 0f;
      while (elapsedTime < expansionDuration)
      {
          light2D.intensity = Mathf.Lerp(0f, maxIntensity, elapsedTime / expansionDuration);
          elapsedTime += Time.deltaTime;
          yield return null;
      }
  
      yield return new WaitForSeconds(0.5f);
  
      elapsedTime = 0f;
      while (elapsedTime < 0.5f)
      {
          light2D.intensity = Mathf.Lerp(maxIntensity, 0f, elapsedTime / 0.5f);
          elapsedTime += Time.deltaTime;
          yield return null;
      }
  
      light2D.intensity = 0f;
  
      isExpanding = false;
  }
}

```
</details>
