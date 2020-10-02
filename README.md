# OpenAI Gym written in pure Rust for blazingly fast performance :rocket:

:warning: :construction: Work in Progress :construction:

This library aims be be as close to the original OpenAI Gym library which is written in Python
and translate it into Rust for blazingly fast performance.
This will make the use of Python unnecessary which is awesome.

If you don't mind Python, check out a gym [wrapper](https://github.com/MrRobb/gym-rs).

## Installation
Put this in your Cargo.toml
```toml
gym_rs = { git = "https://www.github.com/MathisWellmann/gym-rs" }
```

## Example
Here is how you can use the cart_pole environment with a randomly acting agent and rendering enabled:
```rust
/*
Cart Pole Environment using random actions
with rendering of the environment
*/
use gym_rs::{CartPoleEnv, GymEnv, ActionType, Viewer};
use rand::{thread_rng, Rng};

fn main() {
    let mut env = CartPoleEnv::default();
    env.seed(0);

    let mut viewer = Viewer::new(1080, 1080);

    let mut _state: Vec<f64> = env.reset();

    // This produces a different action (random number) with every run,
    // if you would like the agent to behave the same, use Pcg64 with seeding,
    // see env.seed for how it is done in the environment.
    let mut rng = thread_rng();
    let mut end: bool = false;
    let mut total_reward: f64 = 0.0;
    while !end {
        // randomly choose a discrete action to take
        let action = ActionType::Discrete(rng.gen_range(0, 2));
        let (_state, reward, done, _info) = env.step(action);
        end = done;
        total_reward += reward;

        env.render(&mut viewer);
    }
    println!("total_reward: {}", total_reward);
}
```
Run it with:
```
cargo run --release --example cart_pole
```

See [examples](https://github.com/MathisWellmann/gym-rs/tree/master/examples) folder for all the examples


## Contributions are welcome!
If you would like to add an environment or a feature, please fork this repository and create a pull request 
with your changes. Adding new environments should be as easy as translating from Python to Rust. See 
[OpenAI/gym](https://github.com/openai/gym)
for environments that are not yet implemented here! There is alot of easy work to be done here.
Any Help is highly appreciated and benefits the Rust and ML/AI community greatly!

## TODOs:
- implement more environments
- extensive documentation
- compare performance to gym-rs python wrapper
- make generic implementation and compare f32 vs f64 performance.
- publish on crates.io
- make piston_window dependency optional by introducing a render feature if possible

## License
gym-rs is licensed under MIT License just like OpenAI's Gym.

See [LICENSE.md](https://github.com/MathisWellmann/gym-rs/blob/master/LICENSE.md) for further details.
