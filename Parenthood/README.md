# Parenthood

<details>
<summary>BurstLight</summary>
  
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
  private float jumpForce;

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
