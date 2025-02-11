<script lang="ts">
  import { getNumber, getUnit, getRandomColor, getRandomId } from '../utils';
  import { DEFAULTS } from './CONSTANTS';

  const ELEMENTS = 3;
  const SIZE = 80;

  function generateColors(name: string, colors: string[]) {
    const numFromName = getNumber(name);
    const range = colors && colors.length;

    const elementsProperties = Array.from({ length: ELEMENTS }, (_, i) => ({
      color: getRandomColor(numFromName + i, colors, range),
      translateX: getUnit(numFromName * (i + 1), SIZE / 10, 1),
      translateY: getUnit(numFromName * (i + 1), SIZE / 10, 2),
      scale: 1.2 + getUnit(numFromName * (i + 1), SIZE / 20) / 10,
      rotate: getUnit(numFromName * (i + 1), 360, 1),
    }));

    return elementsProperties;
  }

  export let size = DEFAULTS.size;
  export let name = DEFAULTS.name;
  export let square = DEFAULTS.square;
  export let colors = DEFAULTS.colors;

  const properties = generateColors(name, colors);

  const maskId = `mask__marble${getRandomId()}`;
  const filterId = `prefix__filter0_f${getRandomId()}`;
</script>

<svg
  viewBox={'0 0 ' + SIZE + ' ' + SIZE}
  fill="none"
  xmlns="http://www.w3.org/2000/svg"
  width={size}
  height={size}
  data-testid="avatar-marble"
>
  <mask id={maskId} maskUnits="userSpaceOnUse" x={0} y={0} width={SIZE} height={SIZE}>
    <rect width={SIZE} height={SIZE} rx={square ? undefined : SIZE * 2} fill="white" />
  </mask>
  <g mask="url(#{maskId})">
    <rect width={SIZE} height={SIZE} rx="2" fill={properties[0].color} />
    <path
        filter="url(#{filterId})"
      style="mix-blend-mode: overlay;"
      d="M32.414 59.35L50.376 70.5H72.5v-71H33.728L26.5 13.381l19.057 27.08L32.414 59.35z"
      fill={properties[1].color}
      transform={'translate(' +
        properties[1].translateX +
        ' ' +
        properties[1].translateY +
        ') rotate(' +
        properties[1].rotate +
        ' ' +
        SIZE / 2 +
        ' ' +
        SIZE / 2 +
        ') scale(' +
        properties[2].scale +
        ')'}
    />
    <path
        filter="url(#{filterId})"
      d="M22.216 24L0 46.75l14.108 38.129L78 86l-3.081-59.276-22.378 4.005 12.972 20.186-23.35 27.395L22.215 24z"
      fill={properties[2].color}
      transform={'translate(' +
        properties[2].translateX +
        ' ' +
        properties[2].translateY +
        ') rotate(' +
        properties[2].rotate +
        ' ' +
        SIZE / 2 +
        ' ' +
        SIZE / 2 +
        ') scale(' +
        properties[2].scale +
        ')'}
    />
  </g>
  <defs>
    <filter id={filterId} filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
      <feFlood flood-opacity={0} result="BackgroundImageFix" />
      <feBlend in="SourceGraphic" in2="BackgroundImageFix" result="shape" />
      <feGaussianBlur stdDeviation={7} result="effect1_foregroundBlur" />
    </filter>
  </defs>
</svg>
