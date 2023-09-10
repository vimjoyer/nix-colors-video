```nix
.
├── flake.nix
├── home.nix
└── features
    ├── mako.nix
    ├── alacritty.nix
    └── special.nix
# CURRENT DIRECTORY STRUCTURE
```
```nix
{
  description = "main flake";

  inputs = {
    nixpkgs.url = "github:nixos/nixpkgs/nixos-unstable";
    home-manager = {
      url = "github:nix-community/home-manager";
      inputs.nixpkgs.follows = "nixpkgs";
    };

    nix-colors.url = "github:misterio77/nix-colors";

  };

  outputs = { nixpkgs, home-manager, ... }@inputs:
    let
      system = "x86_64-linux";
      pkgs = nixpkgs.legacyPackages.${system};
    in {
      homeConfigurations."vimjoyer" = home-manager.lib.homeManagerConfiguration {
        inherit pkgs;

        extraSpecialArgs = { inherit inputs; };

        modules = [ ./home.nix ];

      };
    };

}
```
```nix
{ pkgs, config, ... }:

{

  services.mako = with config.colorScheme.colors; {
    enable = true;
    backgroundColor = "#${base01}";
    borderColor = "#${base0E}";
    borderRadius = 5;
    borderSize = 2;
    textColor = "#${base04}";
    layer = "overlay";
  };

}
```
```nix
{ pkgs, inputs, ... }:

{
  imports = [
    inputs.nix-colors.homeManagerModules.default
    ./features/mako.nix
    ./features/alacritty.nix
    ./features/special.nix
  ];

  colorScheme = inputs.nix-colors.colorSchemes.gruvbox-dark-medium;
}
```
```nix
{ pkgs, config, ... }:

{
  programs.alacritty.enable = true;
  programs.alacritty.settings = {
    colors = with config.colorScheme.colors; {
      bright = {
        black = "0x${base00}";
        blue = "0x${base0D}";
        cyan = "0x${base0C}";
        green = "0x${base0B}";
        magenta = "0x${base0E}";
        red = "0x${base08}";
        white = "0x${base06}";
        yellow = "0x${base09}";
      };
      cursor = {
        cursor = "0x${base06}";
        text = "0x${base06}";
      };
      normal = {
        black = "0x${base00}";
        blue = "0x${base0D}";
        cyan = "0x${base0C}";
        green = "0x${base0B}";
        magenta = "0x${base0E}";
        red = "0x${base08}";
        white = "0x${base06}";
        yellow = "0x${base0A}";
      };
      primary = {
        background = "0x${base00}";
        foreground = "0x${base06}";
      };
    };
  };
}
```
```nix
{ pkgs, config, ... }:

{
  home.file."mySuperCoolColorValuesFile.xml".text = ''
    <color1>${config.colorScheme.colors.base00}</color1>
    <color2>${config.colorScheme.colors.base05}</color2>
  '';
}
```
